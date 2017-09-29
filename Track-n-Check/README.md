## Some bash commands to track students' repositories and check their homeworks

## 0. Data structure
   I store local copies of my students repositories like this:  
   ```
   cd github_clones
   find . -maxdepth 3 -mindepth 3 -type d
    # ./1/Chelapchij/pikili
    # ./1/Chikalova/kili
    # ./1/Gerasimova/KILIHomeworks
    # ....
  ```
  where 1 is the group number and each student has a folder in which her/his repository(ies) is cloned  
  
  I suggest students to submit their assignments and homeworks in a markdown format (.md, .Rmd, etc.). The files include answer blocks as follows:
  ```
  \```
  # 2.1 
  # 2.2
  \```
  ```
The students write their answers and can add their comments below or above (or elsewhere in the document) as follows:
  ```
  \```
  # 2.1 103,5
  # 2.2 red line
  Actually, I am not sure that...
  \```
  ```
The only thing the students cannot do is to delete or edit the answer line code (e.g. "# 2.1").  
The file name for a given assignment is supposed to be **the same** for all students. Otherwise, their answers will be overlooked by the script.  

I use `grep` (bash command) to collect all the answers and examine them. 
I prefer to quickly look through the answers rather than check them automatically (against a given answer) since we cannot avoid situations when (a) students submit their answers in different formats or (b) there are two or more answers which we cannot predict in advance.  
When in doubt, I can open student's file and read his comments, R/Python code, etc.

## 1. Grep  
This command collects all answers **# 2.1** from multiple homework04.md files  
```
grep -Rn "# 2.1" homework04.md
#
#
...
# ... and append them in the text file output04.txt
grep -Rn "# 2.1" homework04.md >> output04.txt 
```

## 2. Clone 
This command organizes folders in my local folder and make a clone for each remote github repository
```
mkdir 1; cd 1;
mkdir Mokhova; cd Mokhova; 
git clone https://github.com/AnnMokhova/KILI.git;
git add .
cd ..
mkdir Schipkovalina; cd Schipkovalina;
git clone https://github.com/schipkovalina/KILI.git;
git add .
cd ..
...
```
(I usually use a student list and repository index in Excel to compile such commands and create a bash file).  
```
bash mk_clones.sh
```

## 3. Track changes
Starting from the main folder (github_clones), this command makes a list of repository folders ($d) and executes `git pull` in each folder
```
for d in `find . -maxdepth 3 -mindepth 3 -type d -exec ls -d "{}" \;` ; do (cd $d; echo $d; git pull ) done
```
If you see the warning *Your configuration specifies to merge with the ref 'master' from the remote, but no such ref was fetched.*, this means that the repository is probably empty.  
You can log errors in a log file.
```
for d in `find . -maxdepth 3 -mindepth 3 -type d -exec ls -d "{}" \;` ;   
do (cd $d; echo $d; git pull ) done 2>>log`date +%Y%m%d`.txt
```

## 4. Download files
This command downloads a picture from the web site (https://...) and saves it as 4Kopaeva.jpg
```
curl -o 4Kopaeva.jpg https://11kim.github.io/aboutme/img/photo.jpg
# run the script constructed using Excel spreadsheets
sh dump_webpages.sh
```
Find all non-existent web-pages in the folder with downloaded html files:
```
grep -n "404" *.html
```
Grep all lines with img URLs in the file imglist
```
grep -n "<img" *.html > imglist.txt
```



