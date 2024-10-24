# Steps to deploy project on ngnix

Steps:

- navigate to site-available folder and create a file(same name as the site).
- link the file to enabled folder.
- create a folder and place the project the folder inside `/html`.
- Copy and move the folder inside the folder (zip, unzip)
- configure the project file inside `site-available` folder.
- remove default file.
- check by `sudo ngnix -t` command.
- restart ngnix by `sudo systemctl restart ngnix`
