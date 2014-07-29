bjpitts.github.io
=================

personal website

branches
* master
  * current live website
* jekyll
  * working / devel branch
* hyde
  * website created using hyde
* plainHTML
  * website created in plain HTML

updating

    git checkout jekyll
    {make changes}
    jekyll build {test changes}
    git commit -am "BJP: changes made"
    git push origin jekyll
    git checkout master
    git merge jekyll
    git push origin master
