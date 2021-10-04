# Working collaboratively 

Collaboration can involve some or all of the following:

* email exchanges, meetings and discussions
* exchange of code and/or data
* several people working on the same paper/proposal/code/data (perhaps even at the same time)

For papers and proposals there are [Overleaf](overleaf.com) and Google Docs. If you are sharing or collaborating on code or data, it helps if this is done efficiently, since everyone in academia is busy. To this end I created [a few examples](https://github.com/jessecusack/sharing-code) of good and bad practice when sharing code, in the form of an interactive lesson. The good example makes extensive use of `git` and github, which I think are essential tools that every scientist should learn.

Good Practice:

* Store data centrally (cloud storage/server etc.), somewhere all collaborators have access to. This prevents data fragmentation and loss.
* Write good quality code, e.g. copious use of comments, use functions and modules, [sensible variable names](variable_names.md), avoid hard-coded paths, etc. 
* Write reproducible code, e.g. put yourself in the shoes of someone else who wants to run your code - how easy/hard would it be for them? Does it rely on personal functions that only you have access to?
* While working, pull raw data for analysis and push processed data for sharing to and from the central location to your local computer in a reproducible way using scripts. The scripts should, ideally, be under version control and have all their dependencies tracked in some way, such that someone in the future can re-run the analysis/processing with minimal fuss.
* Use git and github for version control/backups as much as possible

Even if you are working alone, following these steps is a good idea because it will make it easier for future you to figure out what past you did!
