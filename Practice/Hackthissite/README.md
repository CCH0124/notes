# Hackthissite
## Basic
### Level: 1
#### URL
http://www.hackthissite.org/missions/basic/1/
#### Exercise
Basic test of your skills to see if you can do any of these missions. Requirements: HTML. This level is what we call "The Idiot Test", if you can't complete it, don't give up on learning all you can, but, don't go begging to someone else for the answer, thats one way to get you hated/made fun of. Enter the password and you can continue.

#### Solution
右鍵查看源代碼。密碼在註解中
==693dc791==

### Level: 2
#### URL
http://www.hackthissite.org/missions/basic/2/
#### Exercise
A slightly more difficult challenge, involving an incomplete password script. Requirements: Common sense. Network Security Sam set up a password protection script. He made it load the real password from an unencrypted text file and compare it to the password the user enters. However, he neglected to upload the password file...
#### Solution
文件尚未上傳，可使用==空密碼登錄==

### Level: 3
#### URL
https://www.hackthissite.org/missions/basic/3/
#### Exercise
This time Network Security Sam remembered to upload the password file, but there were deeper problems than that.
#### Solution
右鍵查看源代碼。有個 type 為 hidden 的 input，其中 value 為 password.php，再將 URL 導向 `https://.../basic/3/password.php` 即可看見密碼

### Level: 4
#### URL
https://www.hackthissite.org/missions/basic/4/
#### Exercise
This time Sam hardcoded the password into the script. However, the password is long and complex, and Sam is often forgetful. So he wrote a script that would email his password to him automatically in case he forgot. Here is the script:
#### Solution
右鍵查看源代碼。有個 type 為 hidden 的 input，其中 value 為 mail 帳號，將此帳號更改成自己的郵件即可會發送一封有關 sam 的訊息，password 為 ==2664d304==

### Level: 5
#### URL
https://www.hackthissite.org/missions/basic/5/
#### Exercise
Sam has gotten wise to all the people who wrote their own forms to get the password. Rather than actually learn the password, he decided to make his email program a little more secure.
#### Solution
右鍵查看源代碼。有個 type 為 hidden 的 input，其中 value 為 mail 帳號，將此帳號更改成自己的郵件即可會發送一封有關 sam 的訊息，password 為 ==8840c634==

### Level: 6
#### URL
https://www.hackthissite.org/missions/basic/6/
#### Exercise
Network Security Sam has encrypted his password. The encryption system is publically available and can be accessed ...
#### Solution
原則上 aaaaaaa 加密後變 abcdefg，a 的 ascii 為 97
||ascii|
|---|---|
|aaaaaaa| 97 97 97 97 97 97 97
|abcdefg| 97 98 99 100 101 102 103
|869ghg=;|56 54 57 103 104 103 61 59
|規則|-0 -1 -2 -3 -4 -5 -6 -7
|答案|56 53 55 100 100 98 55 52

### Level: 7
#### URL
https://www.hackthissite.org/missions/basic/7/
#### Exercise
This time Network Security sam has saved the unencrypted level7 password in an obscurely named file saved in this very directory.

In other unrelated news, Sam has set up a script that returns the output from the UNIX cal command. 
#### Solution
在 Enter the year you wish to view and hit 'view'. 上
輸入 `&& ls`，看到一個名為 k1kh31b1n55h.php 的文件，URL 重新導向 `https://.../basic/7/k1kh31b1n55h.php`，即可得到 ==44ac5770== 密碼

### Level: 8
#### URL
https://www.hackthissite.org/missions/basic/8/
#### Exercise
Sam remains confident that an obscured password file is still the best idea, but he screwed up with the calendar program. Sam has saved the unencrypted password file in `/var/www/hackthissite.org/html/missions/basic/8/`
However, Sam's young daughter Stephanie has just learned to program in PHP. She's talented for her age, but she knows nothing about security. She recently learned about saving files, and she wrote a script to demonstrate her ability.
#### Solution
在 Enter your name: 上
輸入 `<!--#exec cmd="ls ../"-->` SSI 語法，會看到一個名為 au12ha39vc.php 的檔案，將 URL 重新導向 `https://.../basic/8/au12ha39vc.php`，即可得到 ==61468fbe== 密碼
#### 參考
[SSI](https://www.csie.ntu.edu.tw/~b91053/web/exp5/%A4%B0%BB%F2%ACOSSI.htm)
[SSI](https://ssorc.tw/6270)

### Level: 9
#### URL
https://www.hackthissite.org/missions/basic/9/
#### Exercise
Network Security Sam is going down with the ship - he's determined to keep obscuring the password file, no matter how many times people manage to recover it. This time the file is saved in `/var/www/hackthissite.org/html/missions/basic/9/.`

In the last level, however, in my attempt to limit people to using server side includes to display the directory listing to level 8 only, I have mistakenly screwed up somewhere.. there is a way to get the obscured level 9 password. See if you can figure out how...

This level seems a lot trickier then it actually is, and it helps to have an understanding of how the script validates the user's input. The script finds the first occurance of '<--', and looks to see what follows directly after it. 
#### Solution
在第 8 題 Enter your name: 上
輸入 `<!--#exec cmd="ls ../../9"-->` SSI 語法，會看到一個名為 p91e283zc3.php 的檔案，將 URL 重新導向 `https://.../basic/9/p91e283zc3.php`，即可得到 ==42aaa3f0== 密碼
