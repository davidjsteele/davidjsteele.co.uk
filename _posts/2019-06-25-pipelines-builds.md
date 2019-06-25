---
layout: post
title: "Azure DevOps"
date: 2019-06-25
---

#### The pipeline(s) and builds will not be deleted
When tidying up old build pipelines you might have seen the error message....
>One or more builds associated with the requested pipeline(s) are retained by a release. The pipeline(s) and builds will not be deleted.

![Screenshot1](../../../2019-06-25/1.PNG)

Feedback on [dev community](https://developercommunity.visualstudio.com/content/problem/442784/one-or-more-builds-associated-with-the-requested-p.html) discusses the issue in more detail and points out that when releases are deleted that associated builds should have locks removed but this isn't always the case.  I had already removed all releases associated with the builds so we need to remove the lock before the builds can be removed.


### Solution

The solution is to check and set the **RetainedByRelease** flag present against all builds that have been locked by way of a release.
If you call Azure Devops API endpoint for get build details then you will see that the lock hasn't been released. 
https://dev.azure.com/{organisation}/{project}/_apis/build/builds/{buildid}?api-version=5.0) 


![Screenshot2](../../../2019-06-25/4.PNG)


So in order to remove the lock we can call the [PATCH] endpoint passing a JSON payload containing 
```javascript
{
    "retainedByRelease" : false
}
```
This process needs repeated for all locked builds before DevOps will allow you to delete the old build pipeline. 

As a side note when playing around with postman and trying authenticate via Basic authentication, authentication didnt work until I left username blank and just passed the PAT token as the password.  No base64 encoding.

![Screenshot3](../../../2019-06-25/5.PNG)


<section class="resume-section p-3 p-lg-5 d-flex align-items-center" id="education">
    <div class="w-100">
      <h2 class="mb-5">Education</h2>

      <div class="resume-item d-flex flex-column flex-md-row justify-content-between mb-5">
        <div class="resume-content">
          <h3 class="mb-0">University of Colorado Boulder</h3>
          <div class="subheading mb-3">Bachelor of Science</div>
          <div>Computer Science - Web Development Track</div>
          <p>GPA: 3.23</p>
        </div>
        <div class="resume-date text-md-right">
          <span class="text-primary">August 2006 - May 2010</span>
        </div>
      </div>

      <div class="resume-item d-flex flex-column flex-md-row justify-content-between">
        <div class="resume-content">
          <h3 class="mb-0">James Buchanan High School</h3>
          <div class="subheading mb-3">Technology Magnet Program</div>
          <p>GPA: 3.56</p>
        </div>
        <div class="resume-date text-md-right">
          <span class="text-primary">August 2002 - May 2006</span>
        </div>
      </div>

    </div>
  </section>