This article is targeted to the audiance who are working on Backup and Recovery using Veritas NetBackup.

### Requirment
Need to monitor Diskpool status in the NetBackup environment which is having a large count of NetBackup servers.

### End result

We should be receiving a html file having the Diskpool status from all NetBackup primary servers in tabular format with color coded depending on the % of the storage.

### Steps involved
* Gather data of the disk pools and dump locally.
* Extract the data from dump file to required format, in our case tabular form(for now).
* Gather Diskpool dump from remote servers and append to the local tabular data.
* Create a script to add the gathered date to HTML template file in the table format.
* Add java script to color code the table cells depending on the value(%fullof te disk).
* Email the report to the requored audiance.
* Schedule the report using crontab.

## Detailed explaination

* ### Gather Disk status
    * The best way to gather disk information is at volume level using nbdevquery -listdv
    * Gather the disk volume information from all the storage server type(Basic, Advanced, PureDisk, OpenStorage) and save in a file.

    ```
       /usr/openv/netbackup/bin/admincmd/nbdevquery -listdv -stype <storage_type> >> dprawdata

    ```
    * Once you get the raw data of disk volumes, using awk arrays, scrape the data of required fields, in our case Diskpool name, Storege Server type, Total space and available space.

    ```
    # Tabulate the data from raw disk volume file

    awk -v hostname="$(hostname)" '
        NR{
            DISKPOOL[NR] = $2
            STS[NR]=$3
			TOTALSPAC[NR]=$6
			 AVAILPACE[NR]=$7
			 USED[NR]=($6-$7)
			 USD_PERCENT[NR]=($6-$7)*100/$6
        }
        END {	
				printf ("%20s%40s%27s%20s%20s%20s%20s\n", "Master,","DiskPool,", "%Full,", "Total Capacity(TB),", "Used Capacity(TB),", "Available Capacity(TB),","Store Server Type" )
                for(x in DISKPOOL) 
				printf("%20s%0s%40s%0s%27.2f%0s%20.2f%0s%20.2f%0s%20.2f%0s%20s\n", hostname,"," , DISKPOOL[x],"," , USD_PERCENT[x],"," , TOTALSPAC[x],"," , USED[x],"," , AVAILPACE[x],",",STS[x] )
        }'<dprawdata

    ```

    * Calculate used and used percentage of disk space using above data and print the final out put like below to a file(to whihc data from remote servers is appended.)

    ```
            Master,           DiskPool,                     %Full,   Total Capacity(TB),  Used Capacity(TB), Available Capacity(TB),   Store Server Type
             flpd633,         VAp01_fvanbu03med02,          58.27,   188.09,              109.60,              78.50,                  PureDisk
             flpd633,         ODP1_common_ffpddr25,         0.00,    10.86,                0.00,               10.86,                  DataDomain
             flpd633,         SDp1_flph865,                 0.00,    21.28,                0.00,               21.28,                  AdvancedDisk
             flpd633,         SDp1_flph706_brp,             0.16,    12.80,                0.02,               12.78,                  AdvancedDisk
             flpd633,         DP1_p1cvs1d1_ffpddr25,        0.00,    17.32,                0.00,               17.32,                  DataDomain
             flpd633,         SDp1_t1cim1d9,                0.00,    19.28,                0.00,               19.28,                  AdvancedDisk
    ```

    > Above steps need to be performed on all the remote servers(as a single script) and the final out put should be appended to the local file(on which the scipt in initialized).

* ### Gather Disk status from Remote server

    * Above process of collecting disk volume data need to be done on all NetBackup primary servers, so all above steps are consolidated to a single script and placed on all remote servers.
    * Once the scipt is initiated on main server it should trigger all servers to run script and should be able to append the output to local file.
    * To acive that we will use ssh to execute script on remote servers and append output to local file.

    ```
       ssh -t -t -i <private_key> -o StrictHostKeyChecking=no <server_01> ""/usr/openv/netbackup/local/scripts/dpstatus"" >>~/dpout
       ssh -t -t -i <private_key> -o StrictHostKeyChecking=no <server_02> ""/usr/openv/netbackup/local/scripts/dpstatus"" >>~/dpout 
       .
       .
       .
       ssh -t -t -i <private_key> -o StrictHostKeyChecking=no <server_n> ""/usr/openv/netbackup/local/scripts/dpstatus"" >>~/dpout
    ```
    * You can run for loop with the server in a file(Personally I prefer that).
    * With the above step, we have disk volume details of all server in a single local file in a tabular format with "," as delimiter.
    
* ### Tabulate script
    * We have the required data with "," as delimiter on a local file.
    * Our next step would be preparing a template html file where apart of table, rest every thing is static.
    * We can discuss about the tabulate script in other article, but all this script does is insert the table with the data provided from file with a specific delimiter(in our case it's ",").
    * Below is the syntax to insert data from dump file to HTML.
    ```
    cat ~/dpstatus | {cat echo} | tabulatev2.sh -d "," -t "NBU DP Status" -h "Disk Pool Status" >~/dpstatus.html
    ```
    > * "dpstatus" is dump file whihc contains disk volume data from all master servers.
    > * -d specifies the delimiter
    > * -t for title of the html file
    > * -h for header name.
    * With above command, html file is created.
    * Color coding of cells depending on %fullof disk space is one of our requirment, for the we append below JScript to html file created in above step.
    ```
        echo '<script>
      $("td:nth-child(3)").each(function () {
        if (parseInt($(this).text()) >= 85) {
          $(this).addClass("add_red");
        } else if (parseInt($(this).text()) >= 75) {
          $(this).addClass("add_yellow");
        } else if (parseInt($(this).text()) >= 0) {
          $(this).addClass("add_green");
        }
      });
        </script></html>' >>~/dpstatus.html
    ```
    * Which this we will have our final html file which is ready for delivery.







