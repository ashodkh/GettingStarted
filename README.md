# GettingStarted with git and GitHub

<p>
  <ac:macro ac:name="toc"/>
</p>
<p>Please use the comments below to post requests for more FAQ!</p>
<p>You can also ask for help at the DESC GitHub room on <a href="https://confluence.slac.stanford.edu/display/LSSTDESC/Getting+Started+using+HipChat">HipChat</a>, and the <a href="https://help.github.com/">GitHub help pages</a> are very good.</p>
<p> </p>
<hr/>
<h6>What is Git? And GitHub?</h6>
<p>git is a versioning system, like svn but better. It allows you to work offline, committing changes to a local "clone" of the repository, and then pushing them to the remote repository when you get back to wifi. </p>
<p>GitHub is a web service that hosts remote git repositories and enables collaboration via some nice tools. Repositories (or "repos" as they are known on GitHub) can be either public, enabling any of your colleagues to provide feedback or contribute to your project, or private, in case you need to make blind datasets or something. The LSST DESC has an "organization" on GitHub to keep our repos together in one place. It's nice. Here's the <a href="https://github.com/DarkEnergyScienceCollaboration">LSST DESC Organization homepage</a> and here's an <a href="https://github.com/drphilmarshall/Pangloss">example of a repository</a> that you can browse around in.</p>
<p>You will need an account on GitHub: follow <a href="https://github.com">this link</a> and fill in the form, including your full name so that your collaborators can find you easily.</p>
<p>You will also need the unix command git to work on your local machine. </p>
<hr/>
<h6>Slow down. What is a "versioning system"?</h6>
<p>Ah, sorry. Imagine you are working on a document, and you want to save your old versions in case you want to go back to one of them if your plans change, or if your computer breaks down. You'd end up with a series of files called, for example, ms.v1.tex, ms.v2.tex, ms.v3.tex, ms.final.tex, ms.final2.tex, ms.submitted.tex and so on. A versioning system is a computer program that does this for you. It allows you to work on one file, ms.tex, while keeping track of all your old versions. It allows you to go back to them if you want. It also handles that situation where your collaborator makes some changes and sends you ms.v1.pjm.tex, <em>after</em> you have moved on to ms.v2.tex: it <em>merges</em> the two files together for you. Let's compare some basic usage of the git versioning system with your old way of doing things.</p>
<table>
  <tbody>
    <tr>
      <th>Manual versioning</th>
      <th>Git</th>
    </tr>
    <tr>
      <td>mkdir old</td>
      <td>git init</td>
    </tr>
    <tr>
      <td>
        <p>cp ms.tex ms.v1.tex</p>
        <p>mv ms.v1.tex old/</p>
      </td>
      <td>
        <p>git add ms.tex</p>
        <p>git commit -m "Initial version" ms.tex</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>edit: ms.v1.tex</p>
        <p>save as: ms.v2.tex</p>
      </td>
      <td>
        <p>edit: ms.tex</p>
        <p>save</p>
      </td>
    </tr>
    <tr>
      <td>cp ms.v1.tex old/</td>
      <td>
        <p>git commit -m "Finished introduction" ms.tex</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>edit: ms.v2.tex</p>
        <p>save as: ms.v3.tex</p>
      </td>
      <td>
        <p>edit: ms.tex</p>
        <p>save</p>
      </td>
    </tr>
    <tr>
      <td>
        <span>cp ms.v1.tex old/</span>
      </td>
      <td>
        <span>git commit -m "Added references" ms.tex</span>
      </td>
    </tr>
    <tr>
      <td>
        <p>save ms.v1.pjm.tex from email</p>
        <p>edit: ms.v2.tex and ms.v1.pjm.tex</p>
        <p>save as: ms.v3.tex</p>
      </td>
      <td>
        <p>git remote add phil http://phil.com/paper.git</p>
        <pre class="command-line">
          <span style="font-size: 10.0pt;line-height: 13.0pt;white-space: pre-wrap;font-family: Arial , Helvetica , FreeSans , sans-serif;background-color: transparent;">git pull phil master</span>
        </pre>
      </td>
    </tr>
    <tr>
      <td>cp <span>ms.v2.tex ms.v1.pjm.tex old/</span>
      </td>
      <td> </td>
    </tr>
    <tr>
      <td>
        <p>edit: ms.v3.tex</p>
        <p>save as: ms.final.tex</p>
      </td>
      <td>
        <p>edit: ms.tex</p>
        <p>save</p>
      </td>
    </tr>
  </tbody>
</table>
<p>and so on. The magic command that saves you manually combining two files is "git pull phil master". What must have happened here is that your colleague Phil has obtained a copy (a "clone") of the "repository" that you made with "git init". (Before it was just a folder: git init turns it into a repository, with a hidden directory called .git that contains all your old versions - or rather, the differences between old versions - that git can use to reconstruct your past work). Then, Phil has put it on his webserver, so that you can access it remotely. The "git remote add" command links the two repositories (yours and Phil's) together, so that you can each pull in the edits that the other makes. ("master" is the name of the "branch" of the repository that Phil was working in - we'll come back to branches in a second.)</p>
<p>With git (and other versioning systems), the act of archiving your old version is called "committing your changes." It's good to do this often, so that you have more options as to which version to go back to if you need to (because you don't have to worry about out of control file proliferation any more, right?). When you do a git commit you get to make a comment at the same time, to summarize in a few words what you did in this editing round. These comments are summarized for you when you do a "git log". The output of this command looks something like this:</p>
<p> </p>
<pre>commit 95d6aad841215ce21472f68ef766ead9eabec1e7<br/>Author: Your Name &lt;your.name@emailprovider.com&gt;<br/>Date: Thu Jul 2 09:24:28 2015 -0700</pre>
<pre>   Merge branch 'master' of phil.com:paper</pre>
<pre>commit 6c371b736abfb6fead8e15b378ead66675a313f0<br/>Author: Your Name &lt;your.name@emailprovider.com&gt;<br/>Date: Thu Jul 2 10:45:05 2015 -0700</pre>
<pre>   Added references</pre>
<pre>commit 3c431b7236cdfb612ad8e15b378ead66675a32245<br/>Author: Phil &lt;phil@phil.com&gt;<br/>Date: Thu Jul 2 10:17:32 2015 -0700</pre>
<pre>   Wrote method, results, discussion and conclusions</pre>
<pre>commit 6f43fe926fbb23d5c7bfc94ed0f7204387aef918<br/>Author: Your Name &lt;your.name@emailprovider.com&gt;<br/>Date: Thu Jul 2 10:00:05 2015 -0700</pre>
<pre>   Finished introduction</pre>
<pre>commit 3264125999c663ac696f7338fc1252be5551a018<br/>Merge: 06abaac 4853e0a<br/>Author: Your Name &lt;your.name@emailprovider.com&gt;<br/>Date: Thu Jul 2 09:59:47 2015 -0700</pre>
<pre>   Initial version</pre>
<p>Those horrendous hexadecimal strings are "commit IDs" - they are what you need to revert to an old version of your document. Actually, you don't need the whole string, just the first 7 characters. Suppose you want to go back and work on your old version (the one where you added the references but before you merged in the rubbish that Phil wrote). Here's what you would do:</p>
<table>
  <tbody>
    <tr>
      <th>Manual versioning</th>
      <th>Git</th>
    </tr>
    <tr>
      <td>history</td>
      <td>git log</td>
    </tr>
    <tr>
      <td>
        <p>mkdir rewording</p>
        <p>cd rewording</p>
        <p>cp old/ms.v2.reworded.tex .</p>
      </td>
      <td>git checkout 6c371b7 -b rewording</td>
    </tr>
    <tr>
      <td>
        <p>edit: ms.v2.reworded.tex</p>
        <p>save as: ms.v2.reworded.v2.tex</p>
      </td>
      <td>
        <p>edit: ms.tex</p>
        <p>save</p>
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <p>mkdir old</p>
        <p>cp <span>ms.v2.reworded.tex old/</span>
        </p>
      </td>
      <td colspan="1">git commit -m "Better text than Phils" ms.tex</td>
    </tr>
  </tbody>
</table>
<p>Instead of making a new folder (called, eg, "rewording") and working on a reworded version in it, with git you would make a new <em>branch</em> of the repository (called "rewording") and work on ms.tex there. The command for moving between branches (like changing directories) is "git checkout". The initial branch is called "master" - good practice is to use master for the current, best, working version, and all other branches for experimenting. </p>
<p>Now, suppose you want to submit your version of the document to a journal. You talk to Phil, and persuade him that your text is better - not by emailing him your version, but by pushing your new branch to his repository (assuming he gave you permission). Then you can carry on editing in the master branch - which is like going back to your main directory:</p>
<table>
  <tbody>
    <tr>
      <th>Manual versioning</th>
      <th>Git</th>
    </tr>
    <tr>
      <td>cd ../</td>
      <td>git checkout master</td>
    </tr>
    <tr>
      <td colspan="1">pwd</td>
      <td colspan="1">git status</td>
    </tr>
    <tr>
      <td>cd reworded</td>
      <td>git checkout rewording</td>
    </tr>
    <tr>
      <td colspan="1">pwd</td>
      <td colspan="1">git status</td>
    </tr>
    <tr>
      <td>email <span>ms.v2.reworded.v2.tex to Phil</span>
      </td>
      <td>git push phil rewording</td>
    </tr>
    <tr>
      <td colspan="1">discuss, agree</td>
      <td colspan="1">discuss, agree</td>
    </tr>
    <tr>
      <td colspan="1">
        <p>cd ../</p>
        <p>cp rewording/<span>ms.v2.reworded.v2.tex ms.final2.tex</span>
        </p>
      </td>
      <td colspan="1">
        <p>git checkout master</p>
        <p>git merge rewording</p>
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <p>edit: ms.final2.tex</p>
        <p>save as: ms.submitted.tex</p>
      </td>
      <td colspan="1">
        <p>edit: ms.tex</p>
        <p>save</p>
      </td>
    </tr>
    <tr>
      <td>cp ms.final2.tex old/</td>
      <td>git commit -m "Formatted for journal" ms.tex</td>
    </tr>
  </tbody>
</table>
<p>Hopefully this shows something of how git makes keeping track of your changes much simpler. You only ever edit one file, and you only have to do minimal manual editing to merge changes from multiple collaborators ("conflicts" between different versions of the same files do arise, but only when the same lines of the file have been edited, and so they are usually easy to fix - certainly much easier than merging two versions by hand in an editor). Branches take a bit of getting used to: a git checkout can make your current working directory look very different, unlike any other unix command you use! But thinking of it as being like "cd" is helpful. The "git status" command is incredibly useful: it tells you which files have been modified since the last commit, if there are any files that have not yet been added to the repository, if any files have been deleted since the last commit, all as well as which branch you are on.</p>
<p>As you might have guessed, git pull is actually a shortcut to two commands one after the other: git fetch (to get any new commits from the remote repository) and git merge (to merge the files in the remote branch with the current local one). Unlike with doing things by hand, It's actually quite hard to over-write files and lose work. Git will not let you pull in other peoples changes until you have committed yours, and it will not let you push your changes to a remote repository until you have first pulled its changes in and merged them. And finding old versions by your commented history is much easier than trying to remember the meaning of your own filenames!</p>
<hr/>
<h6>How do I contribute to a project on GitHub?</h6>
<p>If you have been given write access to a GitHub repository, you can "clone" it to your local machine and start work. If you have not, you can still contribute by making a "fork" (there's a button for this in the top righthand corner of the GitHub page for each repository). This will make a copy of the repository in your GitHub account, that is linked to the "base repo" - you can then clone from your fork to get the project onto your local machine.</p>
<p>To clone a repo, look down the right hand side bar of its GitHub page. You should see "http clone URL" and a clipboard icon next to it. Under this there is the "SSH" option - select this, and then click on the clipboard. You now have the address of the remote repo in your clipboard. Go to your terminal, and cd to the place where you want your copy of the repo to live (it has its own folder). Then do "git clone &lt;paste&gt;" and hit return.</p>
<p>When you first do this, it will fail. Read the message! Git error messages are almost always very helpful. This one says that your ssh keys need to be set, so let's do that. Go to your profile (the very top right hand corner of the GitHub window, there should be a picture of you) and choose "settings". In the resulting list is an entry called <a href="https://github.com/settings/ssh">"SSH Keys"</a> in the left hand side bar. Go here and paste in your **public** SSH key. This enables GitHub to let you upload files to its server over SSH without typing your GitHub password all the time. If you don't know what an SSH key is, the help links on the SSH keys page you are on are pretty helpful.</p>
<p>Now repeat the git clone command and you should see a local copy of the repo appear.</p>
<hr/>
<p> </p>
<h6>How do I get the latest version of the repository?</h6>
<p>This is typically in the master branch of the base (original) repository, so, after doing a "git status" to make sure you are in the right branch, do "git pull origin master".</p>
<p>If your local repo is a clone of a fork, you'll want to connect it to the base repo with "git remote add upstream ownersname:reponame.git", and then you can pull in changes from the base repo with "git pull upstream master". Don't forget to do "git status" before you pull.</p>
<hr/>
<h6>How do I commit my edits?</h6>
<p>Git has a commit command, just like svn: mostly you will use it as follows: git commit -am "comment"</p>
<p>The '-a' commits all changes. You can see what you are about to commit by doing 'git status'. In fact, you should do a 'git status' before doing anything - it shows you which branch you are on, which files have been added, deleted, modified and so on.</p>
<p>After committing, your edits still only exist in your clone of the repository. To share them with other people you can push them to any other remote repository you have push access to - most commonly, the remote repository at GitHub. When you cloned the repo to your machine, git set up the GitHub repo as your default remote, with the name "origin". After you have committed your changes, you should then do 'git push origin master' - which means "push my work to the master branch of the remote repository origin".</p>
<p>Git will not let you push to a remote repo until you have first updated your local clone with any changes that have been made in the meantime at the remote repo. If you get an error that says as much, do a 'git pull origin master' to pull down the changes from the master branch of the remote repo (named "origin"). </p>
<p>To see all the remotes that you have access to, type 'git remote -v'.</p>
<hr/>
<h6>I git pulled and now I have a conflict. What do I do?</h6>
<p>Fix it. The error message tells you which files contain the conflict. Open them in an editor and search for the string '&gt;&gt;&gt;&gt;&gt;&gt;'. Just like in svn, the portion of code between this string and the '======' mark is the remote version, while the portion below it and above the '&lt;&lt;&lt;&lt;&lt;&lt;' string is your local version. Edit the file so it is correct. Then, to resolve the conflict in &lt;filename&gt;you 'git add &lt;filename&gt;'.</p>
<hr/>
<h6>I want to delete a file. How do I do that?</h6>
<p>Just rm it as usual, and then do 'git status'. You'll see that git understands file deletion: when you commit all your changes, git will stop tracking that file. You'll still be able to access old versions of that file in the repository, though.</p>
<hr/>
<h6>I made some edits that I don't like and want to go back to the original file. What do I do?</h6>
<p>If you haven't committed your edits you can just git checkout – &lt;file&gt; and you will get back the original file. Be warned that your edits on this file will be lost (it will be overwritten)</p>
<hr/>
<h6>What's the best way to make a new repository?</h6>
<p>If you have admin access (ie you are in the DESC Leadership), you can go to the <a href="https://github.com/DarkEnergyScienceCollaboration">DESC GitHub page</a> and click on the big green button. You can make your own repos on your <a href="https://github.com/">GitHub home page</a>, again with the big green "New repository" button.</p>
<p>To turn one of your existing folders into a git repository, just do "git init" and then start git add'ing files. If you later want to push this to GitHub, you'll still need to start a repo on the GitHub site - just don't initialize it with a README or anything, just start it and then pick up its address (the thing that ends with ".git"). Then, on the command line, add a link to this new remote repository with "git remote add origin &lt;address&gt;". Then you can push to it as normal. More instructions <a href="https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/">here</a>.</p>
<p>It's best to initialize a repo with a README (so you can tell people what the project is about) and a license file (so everyone is clear about what you are happy for people to copy and re-use) but you don't have to. A .gitignore is useful though - it tells git to ignore certain files and filetypes, so that they don't clutter up your git status messages. Once the repo has been started, you can then clone it to your local machine.</p>
<p>In the repo's settings, at the bottom of the righthand sidebar, you can add collaborators (giving them read, write or admin access), and turn on the wiki associated with the repo, if you want.</p>
<hr/>
<h6>How do I push and pull without having to type my password all the time?</h6>
<p>You can give GitHub your public SSH key instead. See the instructions <a href="https://confluence.slac.stanford.edu/display/LSSTDESC/Getting+Started+with+Git+and+GitHub#GettingStartedwithGitandGitHub-HowdoIcontributetoaprojectonGitHub?">above.</a> </p>
<hr/>
<h6>What is a GitHub "issue"?</h6>
<p>When coding, many issues arise that need to be addressed: bugs, new features that you want, questions you have about the documentation and so on. When you have identified an issue, you usually want to do two things: 1) make a note of it so you can deal with it later and 2) tell your collaborators about it. GitHub issues do both.</p>
<p>To start a new issue, go to the circle with an exclamation point inside it in the repo's right hand sidebar (right under "code" and above "Pull requests").  Then, hit the big green "New issue" button, give it a title (like the subject line of an email, summarizing the issue) and if necessary, a short description of what needs to be done - and when you hit submit, the issue is added to the repo's list, and a notification email is sent to everyone who is "watching" the repo. This is a Good Thing: you want to be able to keep up with your projects! You can give it a try at <a href="https://github.com/drphilmarshall/scriptutils">this old repo here</a>. To "watch" a repository, and hence follow its issues, click on the "Watch" button in the top right hand corner of the repo's page.</p>
<p>Any other GitHub user can watch your repo <span>(and hence follow its issues)</span>, as long as it is public not private.  They can also submit issues. This is a Good Thing: it provides a means for anyone to give you feedback about your project, and lets everyone know what you are working on so they can avoid wasting their time duplicating effort. Private repos also have issue lists attached to them, but only the people in that repo's collaborator list can see them. To adjust the private/public nature of a repo,  and adjust its collaborator list, go to the repo's "settings" via the spanner/screwdriver icon in the right hand sidebar.</p>
<hr/>
<h6>What is a "Pull Request"?</h6>
<p>Suppose you see something that needs fixing in a repo's code. Here's a good way to go about fixing it: 1) Make a branch to contain the fixed code, with something like "git checkout -b betterlayout" . 2) Edit the code and commit and push your changes, with "git push origin betterlayout". This makes a corresponding branch, also called "betterlayout" on the remote repo "origin". 3) Go to the repo's page on GitHub. It will probably prompt you to "submit a pull request" - if it doesn't, select the "betterlayout" branch from the "branch:" menu next to the repo name. 4) Click on the button to start a pull request. An issue-like form will appear, where you can edit the title of the pull request (eg "Better LaTeX Layout?") and provide some comment on what you have done and why. 5) Submit the pull request with the button at the bottom of the form. This will notify the repo's owner, and everyone else who is watching the repo, that you have made some changes and would like them to be merged into the code. The owner will then review your changes - notice how all the commits that have been made in the "betterlayout" branch are tracked automatically in the pull request thread.</p>
<p>As you can see, a pull request is a request for your changes to be pulled into another branch of the repository, typically the master branch. You often see repos with READMEs that say "pull requests welcome!" This is because they provide a mechanism for anyone to add value to your project  by making improvements and then asking you to accept them! As owner, you don't have to accept any pull request, but usually they are a Good Thing. And you always get to review them first anyway.</p>
<p>Notice that you can submit a pull request from any branch, including a "fork" of the repository - if you don't have push access to the base repository, just fork it, edit it, and submit a pull request from there. Just keep reading the messages closely to see what is going on.</p>
<hr/>
<p> What's the difference between a "Fork" and a "Branch"?</p>
<p>A fork is a clone of the repository, in a different GitHub user's account. It comes with a master branch, and can have multiple additional branches just like any other repo. One key feature of a forked repo is you can push commits to it, even if you do not have push access to the base repo.  Another is that GitHub knows that the fork is connected to the base repo - and it makes it easy for you to submit a pull request from eg the master branch of the forked repo to the master branch of the base repo. </p>
<p>As soon as you fork a repository, have in mind that it is continually diverging from the base repo - because even if you are not editing the code, someone else might be! To keep your forked repo up to date, you'll need to pull in changes from the base repo from time to time. Here's what you do: 1) clone your fork with "git clone yourname:thereponame.git" as usual. This makes a local copy of the repo, and attaches the name "origin" to the remote fork at GitHub. 2) Connect your local clone to the base repo, with "git remote add upstream ownersname:thereponame.git". To see which remotes you have defined, do "git remote -v" 3) Pull in updates with eg "git pull upstream master" (which merges commits made to the master branch of the owner's repository - the base repo - into your current branch). Don't forget to do a "git status" to make sure you are in the right branch before pulling! </p>
<hr/>
<h6>Where can I find out more?</h6>
<ul>
  <li>The <a href="https://help.github.com/">GitHub help pages</a>
    <span> are good.</span>
  </li>
  <li>
    <span>You can listen to Phil introducing git and GitHub to some DESC members on youtube <a href="https://www.youtube.com/watch?v=tC5-ndM_MhE">here</a>. </span>
  </li>
  <li>GitHub <a href="https://docs.google.com/document/d/1fVIbkb0moMSrRFhatDIl4_o5GB1duh9w2Y5t7EhEZws/edit?usp=sharing">Quick Start Guide</a> meant to get you started pulling and pushing within 10 minutes. Just the basics. Includes both GUI and Command Line introduction. Written by Karen Ng and Will Dawson. </li>
  <li>
    <a href="http://git-scm.com/book/en/v2">Pro Git Book</a>: The definitive resource on everything git.</li>
  <li>
    <a href="https://try.github.io//levels/1/challenges/1">Code School</a>: Git. Nice interactive way to learn the basics of git in ~15 minutes.</li>
  <li>
    <a href="https://confluence.lsstcorp.org/display/SIM/Git+and+STASH+for+Simulations">LSST Simulations Framework Guide</a>. For the more sophisticated command line users. Written by Mario Juric and Andrew Connolly.</li>
  <li>The branching nature of git can be tricky to visualize at first.  These <a href="http://onlywei.github.io/explain-git-with-d3/">visual git tutorials</a> are helpful to understand what the git commands are doing in the git commit tree.</li>
</ul>
<p> </p>