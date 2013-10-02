=========
GIT NOTES
=========

Get a file from a sha1
----------------------

Solution [1]_

::
    git checkout 08618129e66127921fbfcbc205a06153c92622fe -- [full/path]

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
