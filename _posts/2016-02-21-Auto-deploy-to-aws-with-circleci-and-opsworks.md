---
layout: post
title: Auto deploy to AWS with CircleCI and Opsworks
date: 2016-02-21 19:13:00.000000000 +01:00
tags: AWS
---

![please deploy](https://cloud.githubusercontent.com/assets/775611/13204595/1b16fa26-d8d4-11e5-81e2-5ca93371ab15.png)

When we develop a feature, eventually we will want to see it running on a staging
server and test it, or simply the code reviewer would like to do the same.

The old way is to `git pull` our branch on the staging server, later we have opsworks,
much better, but still, have to click a few buttons to have it done.

Then I finally found a way to auto deploy my feature branch when all tests passed
on CircleCI. So in the `circle.yml` file you can write:

{% highlight yml%}
deployment:
  staging:
    branch: /feature\/.*|develop/
    commands:
      - aws opsworks update-app --region <region-name> --app-id <app-id> --app-source Revision="${CIRCLE_BRANCH}"
      - aws opsworks create-deployment --region <region-name> --app-id <app-id> --command "{\"Name\":\"deploy\"}" --comment="CircleCI deploy staging"
{% endhighlight %}

that means when some code in `develop` branch or any feature branch, if code passed
tests, update the app's revision on aws firstly, then deploy it. Yes, this simple :)

related docs can be found at [cicle](https://circleci.com/docs/introduction-to-continuous-deployment),
[awscli update-app](http://docs.aws.amazon.com/cli/latest/reference/opsworks/update-app.html) and
[awscli create-deployment](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html).
