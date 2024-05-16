---
title: "Hosting NextJS project on Vercel"
subtitle: "Hosting NextJS application on Vercel with custom domain "
date: "2024-05-16"
---

In this article we will see how to host NetxtJS application(in my case blogging site) on Vercel with custome domain name.

## Getting started

> If you have a NextJS project that you created as a hobby, you can host for free on Vercel. If you own a custom domain name you can use that domain name too.

### Requirments

* NetxtJS application
* Vercel account
* Github Account
* Custome domain(paid and optional)

## Steps to host NextJS application on Vercel

* Maintain a repository of NetxtJS application on Github.
* Create a account on Vercel(preferable with github).
* Grant access to the NextJS application repository.
* Configure build operation.
* Validate the build and visit website.
* Add custome domain.

### Create a NextJS app repository.

* Create a github account if you doesn't have.
* create a repository with NextJS application.

![Repository Image](/images/github_repo.png)

### Create a account on Vercel and import repository.

* Sign up for the Vercel account with GitHub, [Vercel account sign up](https://vercel.com/signup).
* Select the NextJS application repository and import to Verce.
* Proceed with the promps and use NetxtJS as deployment.
* Once you click deploy, application starts building and you can see the build logs.

![Vercel Build](/images/vercel_build_logs.png)

* Once deployment is completed you can visit site or add domain.

* You can share the URL created for friends and it is accessable for public.

### Adding custome domain

* If you own a custome domain, you can use it for the hosted NextJS app on Vercel.
* Go to domains page and add your custom domain and click add.
* Update the Domain nameservers on the domain provider.

![Nameserver Update](/images/nameservers_on_domain.png)

* Once the nameservers are updated on domain provider, domain validation on Vercel should be completed like below.

![Domain Update](/images/vercel_domain_update.png)

*  Once domain is verified, you can visit your domain name enjoy your NextJS application.

## Conclusion

> Following above steps, we can host netxtJS application on Vercel for free.
Website will be updated with new build when a new commit is done to the github repository 🎉





