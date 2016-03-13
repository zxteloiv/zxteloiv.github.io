---
layout: post
title:  "Best Practice for virtualenv and git repo"
date:   2016-03-13 12:00:00 +0800
tags: python virtualenv github
categories: main learning
---

Although this is a very basic topic in python, I have just learnt it and thus write some memo here.

There're many cases one might use virtualenv, e.g., you maintain several python distributions on a single computer, or you want to distribute your own package and don't want to package the dependencies. Anyway, virtualenv gives a good way to manage all the directories a python distribution uses. Moreover, virtualenv won't bother you with your environment at hand. It's just an additional or bonus function for you. You don't need to move all your working style into a virtualenv way.

Let's come to the point.

### virtualenv

To install and use virtualenv is simple.

~~~ shell
# install
pip install virtualenv [--user]

# create an env
virtualenv myenv
virtualenv -p /usr/local/bin/pypy myenv # using the pypy distribution

# use the env
source myenv/bin/activate

# exit the env
deactive # which is usable only after you activate the env
~~~

Note that you don't have to manually designate a new pip or new virtualenv in order to use pypy. PyPy will [work well](http://pypy.readthedocs.org/en/latest/install.html#installing-using-virtualenv) if you just set the route as above.

Note again in the above script, we runs pip just as a terminal script. That is environment-dependent. In my case, I'm using Python 2.7.10 from the official site and therefore the pip is cooperative with Python 2.7, which is found in my \$PATH variable. I'm not sure how would it work if both python 2 and 3 are at hand. But pypy have a python-3-compatible version, things would be OK if you use all pip3, pypy3 and python3.

### env management

Actually a virtualenv folder has nothing to do with your python source code folder. You can put the two in any place. But I think to put all _env_ at the same place like `~/.envs` and all python projects elsewhere would be a good choice. It's easier to type `source ~/.envs/myenv/activate` for example.

I don't want to use [virtualenv wrapper](http://docs.python-guide.org/en/latest/dev/virtualenvs/#virtualenvwrapper) as it's not written in python and thus more tedious to setup (you should export many shell variables rather than just give some optional arguments with virtualenv).

### Git repos

When you want to distribute or simply open source your python codes, the good choice is to use `pip freeze > requirements.txt` to get a dependency file. You can add `requirements.txt` into git version control. In order to get the simplest dependency requirements file, you'd better use a virtualenv for all the development.
But the directories created by virtualenv, e.g., `bin`, `lib`, `src`, are better not to put into git repos and not to write into _.gitignore_ file.

If you dig into the advanced topics of pip, the _requirements.txt_ file could also contain lines from VCS like git and hg, which means you can write a _requirements.txt_ containing your common modules from your own repo, when you don't want to or just not yet put them into pypi.

If you refer to the _.gitignore_ file template in GitHub for a python project, you can find a _env_ item. I think you could also put your env in your repo as it's on your own.
Actually if you clone a repo from others and they give a _requirements.txt_ file, you may choose to create an virtualenv just inside the repo directory as it is somewhat temporary.

### Conclusion

- create env in place like `~/.envs/`, or in repo folder but add `env` to `.gitignore`
- add `requirements.txt` to every repo's VCS
- write modules or git repos you usually use into `requirements.txt` and publish as a gist if you like
- no `bin`, `lib`, `src` in _.gitignore_ file or repo


