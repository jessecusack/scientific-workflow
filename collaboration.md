# Working collaboratively 

Collaborations happen with different intensities from low level email exchanges, meetings and discussions, to exchanges of code and data up to furiously working on the same paper/proposal/code/data with several people simultaneously.

For papers and proposals there are [Overleaf](overleaf.com) and Google Docs, which are relatively intuitive and easy to learn. 

What about collaborations on analysis code and software? The first thing to say is that it needs to be done efficiently, since everyone in academia is busy. Github is a great tool for facilitating code collaboration (you can use it for papers and proposals too, so long as all authors know how!). In fact, I think are git and Github are now essential tools that every scientist should learn. However, all users of these tools need to write good code to work effectively (also true in general). I created [a few examples](https://github.com/jessecusack/sharing-code) of good and bad practice when sharing code, in the form of an interactive lesson and summarise the main points below. 

Good Coding/Data Practice:

* Store data centrally (cloud storage/server etc.), somewhere all collaborators have access to. This prevents data fragmentation and loss.
* Write good quality code, e.g. copious use of comments, use functions and modules, [sensible variable names](variable_names.md), avoid hard-coded paths, etc. 
* Write reproducible code, e.g. put yourself in the shoes of someone else who wants to run your code - how easy/hard would it be for them? Does it rely on personal functions that only you have access to? Are there lots of manual steps that are hard to reproduce?
* While working, pull raw data for analysis and push processed data for sharing to and from the central location to your local computer in a reproducible way using scripts. The scripts should, ideally, be under version control and have all their dependencies tracked in some way, such that someone in the future can re-run the analysis/processing with minimal fuss.
* Use git and github for version control/backups as much as possible

Even if you are working alone, following these steps is a good idea because it will make it easier for future you to figure out what past you did!
