# Simple toolchain tutorial flow

Goal: 15 minutes to a *basic* understanding of using toolchains with Bluemix.
Sample project: https://github.com/HadarH/simple-toolchain

Prereqs:
* Bluemix account
* A web browser


1. Introduce toolchains and the Garage Method
    1. Caveat: I suppose? I don't believe that it's especially practical to weave the GM into a short tutorial like this. You're asking people to learn Toolchains + the Garage Method, which are different things.
2. Open and clone sample
    1. Explain what this is:
      1. Express server (Node) running on Bluemix
      2. Simple HTML + CSS page
      3. GH provides Git repo and issue tracking
      4. Web IDE (available, but not required to work with project)
      5. Delivery Pipeline for CI/CD.
      6. These are all tools that can support Garage Method practices...
    2. Explain what is happening behind the scene during cloning. More specifically:
      1. Bluemix apps consist of built code--not necessarily all code in repo
      2. Remote repo != Bluemix space + org != Web IDE local repo
2. Examine and tweak sample
    1. (Optional) Edit static page.
    2. Commit change.
    3. Deploy app. (Do we need to add a deploy stage to the final version of the toolchain?)
    4. Notice the pipeline churning? That's downtime! Let's fix it. (Or something to this effect.)
3. Expand toolchain
    1. Add something to toolchain. Hopefully AD, but maybe Timeline Analytics?
    2. See new tool on overview. 
4. Modify pipeline
    1. Rename deploy stage
    2. Add jobs for AD (I hope there's improved integration in the works--a bit clunky for a quick tutorial).
    3. Briefly explain what those jobs do. 
5. Edit sample
    1. Edit static page
    2. Commit
    3. Observe pipeline
    4. As pipeline deploy starts, have user open HelLo World page. It's up!
    5. Refresh page as deploy finishes to see change. 
6. Discuss next steps with some more depth than in other docs
    1. Discuss Garage Method and link out to a **good** overview--I don't know if this truly exists, yet.
    2. Link to other toolchain projects that demonstrate getting real work done
    3. Link to a few popular toolchain projects
    4. Link to Bluemix