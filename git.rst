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


.. [1] http://stackoverflow.com/a/4218823/465270
.. [2] https://help.github.com/articles/changing-a-remote-s-url
.. [3] http://stackoverflow.com/a/1895095
