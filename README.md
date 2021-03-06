# hbars - create horizontal bar in Bash script
The **_hbar_** script can be used to create text based or web page horizontal bars.

![](hbar0.png)
![](hbars_web0.png)

## Installation and Usage
To install and use the Bash script:
```bash
wget https://raw.githubusercontent.com/metcalfepete/hbars/main/hbars
chmod +x hbars
# to make it available 
sudo mv hbars /usr/bin
```
The script accepts both piped and command data
```bash
$ hbars
usage: hbars [option] [data]
  -h --help     print this usage and exit
  -c --color    set color to all bars (default 7=white)
                 (0-grey,1-red,2=green,3=yellow,4=blue,5=magenta,6=cyan,7=white)
  -p --pretty   add different colors to bars (-c overrides)
  -t --title    top title
  -w --width    max width of bar (default 50)
  -W --Web      make output HTML formatted
  -f --fontsize fontsize for web output (default 24)

 examples:
   echo 'temp,33,C;pressure,14,psi' | ./hbars -t Weather -p -w 40 
   ./hbars -t Weather -p -w 40  'temp,33;pressure,14' 
   cat data.txt | ./hbars -W -f 24 -t 'Raspi Data' > data.htm

```
### Options Arguments:

-c -- color, the color can be set for all bars. The color is based on the ANSI color settings:
         0-grey,1-red,2=green,3=yellow,4=blue,5=magenta,6=cyan,7=white
         (default 7=white)
-p -- pretty, each bar is given a unique color in the order of 1,2,3... 
-t -- title. A string. For web page HTML tags can be include: 
         Web example: -t "<center style='fore-ground:red'>Some Bars</center>"
-w --width, width of bar. Max value is scaled to this width value (default 50)
-W --Web, output is HTML formatted. Output show be directed to a file
-f --fontsize, fontsize for web output (default 24)

### Examples:
The data structure is: label, value, (units - optional), (color -optional);
Each item is separated withhu ',' and each item group is terminated with ";".

An example with data on the command line:
```bash
$ $  hbars -t Weather  -w 40 -t "Weather Data" "temp,33;humidity,80"

            Weather Data

       temp ████████████████ 33 

   humidity ████████████████████████████████████████ 80 

```
At example of data piped in:
```bash
$ echo 'temp,33,C;pressure,14,psi' | hbars -t Weather  -w 40 

            Weather

       temp ████████████████████████████████████████ 33 C

   pressure ████████████████ 14 psi
```



## Background

To create a static bar an *printf* statements can be used. The seq {0..10} can be used to repeat an ASCII '█' fill character 10 times.
```bash
$ printf 'LABEL: ' ; \
  printf '█%.0s' {1..10} ; \
  printf ' 10\n'

LABEL: ████████████████████ 10
```
Unfortunately the printf statement has some limitations on variable substitution. A simple workaround is to create a string and then eval it:
```bash
$ label="temp"; val=20;
$ bar="printf '█%.0s' {1..$val}" ; 
$ printf '\n%-5s ' $label; eval $bar ; printf ' %d\n' $val

temp  ████████████████████ 20
```

## Colored Bar

The **_tput setaf_** command can change the foreground, and tput setab is used for background colours. Colour codes are:
```text
tput setab [1-7] # Set the background colour using ANSI escape
tput setaf [1-7] # Set the foreground colour using ANSI escape

Num  Colour    #define         R G B

0    black     COLOR_BLACK     0,0,0
1    red       COLOR_RED       1,0,0
2    green     COLOR_GREEN     0,1,0
3    yellow    COLOR_YELLOW    1,1,0
4    blue      COLOR_BLUE      0,0,1
5    magenta   COLOR_MAGENTA   1,0,1
6    cyan      COLOR_CYAN      0,1,1
7    white     COLOR_WHITE     1,1,1
```
To reset colours back to the defaults use: **_tput sgr0_**
