=========
GIT NOTES
=========

Get a file from a sha1
----------------------

Solution [1]_

::
    git checkout 08618129e66127921fbfcbc205a06153c92622fe -- [full/path]

Remove commit that has not been pushed
--------------------------------------

Solution [3]_

::
    # reset the index to the desired tree
    git reset f2d3c3fc5442071f5c39326fd286b7baa6736c84

    # Update working copy to reflect the new commit
    git reset --hard

Revert commit 
-------------

Solution [3]_

::
    # move the branch pointer back to the previous HEAD
    git reset --soft HEAD@{1}

    git commit -m "Revert to 56e05fced"

Change Remote branches from https to git
----------------------------------------

Changing remote branches from https urls to git urls allows for password-less
login via SSH (github)

Solution [2]_

::
    git remote -v
    # View existing remotes
    # origin  https://github.com/user/repo.git (fetch)
    # origin  https://github.com/user/repo.git (push)

    git remote set-url origin git@github.com/user/repo.git
    # Change the 'origin' remote's URL

    git remote -v
    # Verify new remote URL
    # origin  https://github.com/user/repo2.git (fetch)
    # origin  https://github.com/user/repo2.git (push)

View Stash
----------

Solution [4]_

::
    git stash show stash@{i}

    git stash show -p stash@{i}

Figure out if a branch has been merged
--------------------------------------

You can get the SHA1 of the commit where two branches "branched" from
eachother. Then you can view the commit for further information about if that
was a merge or not::

    git merge-base origin/feature_branch origin/develop
    > a12a3fcc2bf96f08d8e3c9caee0e98343b79e17d

    git ls-remote origin feature_branch
    > a12a3fcc2bf96f08d8e3c9caee0e98343b79e17d	refs/heads/feature_branch

    git log -1 a12a3fcc2bf96f08d8e3c9caee0e98343b79e17d
    > commit a12a3fcc2bf96f08d8e3c9caee0e98343b79e17d
    > Author: Tom Leo <tom@tomleo.com>
    > Date:   Fri Nov 23 23:59:22 2012 -0500
    >
    >    <Commit Message goes here>


.. [1] http://stackoverflow.com/a/4218823/465270
.. [2] https://help.github.com/articles/changing-a-remote-s-url
.. [3] http://stackoverflow.com/a/1895095
.. [4] Forgot to record the link
