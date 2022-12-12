# CODIGOS DE LINUX

## #6 Validating Floating-Point Input


**Explicacion del codigo:** El proposito del codigo es saber si el usuario esta ingresando o no un numero flotante, esto a traves de una validacion que consiste en validar si dos numeros separados por un punto son enteros (usando como inspiracion el codigo anterior). En caso de que ambos numeros sean enteros y el valor despues del punto no contenga un valor negativo o algo fuera de lo normal enonces se devolvera un 0 como prueba de que el numero decimal ingresado sea correcto.


>Codigo a corregir:
```sh
#!/bin/bash
# validfloat--Tests whether a number is a valid floating-point value.
# Note that this script cannot accept scientific (1.304e5) notation.
# To test whether an entered value is a valid floating-point number,
# we need to split the value into two parts: the integer portion
# and the fractional portion. We test the first part to see whether
# it's a valid integer, and then we test whether the second part is a
# valid >=0 integer. So -30.5 evaluates as valid, but -30.-8 doesn't.
# To include another shell script as part of this one, use the "." source
# notation. Easy enough.
. validint
validfloat()
{
 fvalue="$1"
 # Check whether the input number has a decimal point.
 if [ ! -z $(echo $fvalue | sed 's/[^.]//g') ] ; then
 # Extract the part before the decimal point.
 decimalPart="$(echo $fvalue | cut -d. -f1)"
 # Extract the digits after the decimal point.
 fractionalPart="${fvalue#*\.}"
 # Start by testing the decimal part, which is everything
 # to the left of the decimal point.
The Missing Code Library 27
 if [ ! -z $decimalPart ] ; then
 # "!" reverses test logic, so the following is
 # "if NOT a valid integer"
 if ! validint "$decimalPart" "" "" ; then
 return 1
 fi
 fi
 # Now let's test the fractional value.
 # To start, you can't have a negative sign after the decimal point
 # like 33.-11, so let's test for the '-' sign in the decimal.
 if [ "${fractionalPart%${fractionalPart#?}}" = "-" ] ; then
 echo "Invalid floating-point number: '-' not allowed \
 after decimal point." >&2
 return 1
 fi
 if [ "$fractionalPart" != "" ] ; then
 # If the fractional part is NOT a valid integer...
 if ! validint "$fractionalPart" "0" "" ; then
 return 1
 fi
 fi
else
 # If the entire value is just "-", that's not good either.
 if [ "$fvalue" = "-" ] ; then
 echo "Invalid floating-point format." >&2
 return 1
 fi
 # Finally, check that the remaining digits are actually
 # valid as integers.
 if ! validint "$fvalue" "" "" ; then
 return 1
 fi
fi
 return 0
}
```

>Codigo corregido:
```sh
#!/bin/bash

validfloat()
{
  fvalue="$1"


  if [ ! -z $(echo $fvalue | sed 's/[^.]//g') ] ; then


    decimalPart="$(echo $fvalue | cut -d. -f1)"


    fractionalPart="${fvalue#*\.}"



    if [ ! -z $decimalPart ] ; then
 
      if ! validint "$decimalPart" "" "" ; then
        return 1
      fi 
    fi

 
    if [ "${fractionalPart%${fractionalPart#?}}" = "-" ] ; then
      echo "Invalid floating-point number: '-' not allowed \
        after decimal point" >&2         
      return 1
    fi 
    if [ "$fractionalPart" != "" ] ; then 
   
      if ! validint "$fractionalPart" "0" "" ; then
        return 1
      fi
    fi
  else 
   
    if [ "$fvalue" = "-" ] ; then
      echo "Invalid floating-point format." >&2 ; return 1
    fi

   
    if ! validint "$fvalue" "" "" ; then
      return 1
    fi
  fi

  return 0
}

if validfloat $1 ; then
  echo "$1 is a valid floating-point value"
fi

exit 0
```



## #8 Sidestepping Poor echo Implementations
**Explicacion del codigo:** Se desea remplazar el uso de "-n" con una funcion creada a partir de la misma logica la cual nos facilita la escritura del codigo para que se remplace el comando echo -n con echon para despues poder llamar esta funcion y que realice exactamente lo mismo.

>Codigo a corregir:

```sh
echon()
{
 echo "$*" | awk '{ printf "%s", $0 }'
}
```
**Explicacion de lo que se cambio:** No hubo cambio alguno
>Codigo corregido
```sh
echon()
{
  echo "$*" | awk '{ printf "%s", $0 }'
}
```


## #11 ANSI Color Sequences
**Explicacion del codigo:** Se desea simplificar los codigos de color para que al usuario le sea mas sencillo reconocer dichos codigos. esto a traves de guardar el codigo de color en una variable la cual tenga terminacion en f para la letra y en b para el fondo ademas de tambien poder cambiar el formato de la escritura como por ejemplo a **negrita** o *italica*

>Codigo a corregir:

```sh
#!/bin/bash

initializeANSI()
{
 esc="\033" # If this doesn't work, enter an ESC directly.
 # Foreground colors
 blackf="${esc}[30m"; redf="${esc}[31m"; greenf="${esc}[32m"
 yellowf="${esc}[33m" bluef="${esc}[34m"; purplef="${esc}[35m"
 cyanf="${esc}[36m"; whitef="${esc}[37m"
The Missing Code Library 41

 # Background colors
 blackb="${esc}[40m"; redb="${esc}[41m"; greenb="${esc}[42m"
 yellowb="${esc}[43m" blueb="${esc}[44m"; purpleb="${esc}[45m"
 cyanb="${esc}[46m"; whiteb="${esc}[47m"
 # Bold, italic, underline, and inverse style toggles
 boldon="${esc}[1m"; boldoff="${esc}[22m"
 italicson="${esc}[3m"; italicsoff="${esc}[23m"
 ulon="${esc}[4m"; uloff="${esc}[24m"
 invon="${esc}[7m"; invoff="${esc}[27m"
 reset="${esc}[0m"
}
```

>Codigo corregido:
```sh
#!/bin/bash


initializeANSI()
{
  esc="\033"   

  blackf="${esc}[30m";   redf="${esc}[31m";    greenf="${esc}[32m"
  yellowf="${esc}[33m"   bluef="${esc}[34m";   purplef="${esc}[35m"
  cyanf="${esc}[36m";    whitef="${esc}[37m"
  

  blackb="${esc}[40m";   redb="${esc}[41m";    greenb="${esc}[42m"
  yellowb="${esc}[43m"   blueb="${esc}[44m";   purpleb="${esc}[45m"
  cyanb="${esc}[46m";    whiteb="${esc}[47m"


  boldon="${esc}[1m";    boldoff="${esc}[22m"
  italicson="${esc}[3m"; italicsoff="${esc}[23m"
  ulon="${esc}[4m";      uloff="${esc}[24m"
  invon="${esc}[7m";     invoff="${esc}[27m"

  reset="${esc}[0m"
}
```


## #13 Debugging Shell Scripts

**Explicacion del codigo:** En este codigo lo que se intenta recrear es el clasico juego de adivinar el numero secreto escondido entre un rango dado de numeros. Se planea que el usuario adivine un numero aleatorio(Esto empleando el comando RANDOM) entre el 1 y el 100 y se muestre el numero de intentos que le costo adivinar el numero
>Codigo a corregir:

``` sh
#!/bin/bash
# hilow--A simple number-guessing game
biggest=100 # Maximum number possible
guess=0 # Guessed by player
46 Chapter 1
guesses=0 # Number of guesses made
 number=$(( $$ % $biggest ) # Random number, between 1 and $biggest
echo "Guess a number between 1 and $biggest"
while [ "$guess" -ne $number ] ; do
 /bin/echo -n "Guess? " ; read answer
 if [ "$guess" -lt $number ] ; then
 echo "... bigger!"
 elif [ "$guess" -gt $number ] ; then
 echo "... smaller!
 fi
 guesses=$(( $guesses + 1 ))
done
echo "Right!! Guessed $number in $guesses guesses."
exit 0
```

>Codigo corregido:

```sh
#!/bin/bash


biggest=100                   
guess=0                       
guesses=0                     
number=$(( $RANDOM % $biggest + 1 ))    

echo "Guess a number between 1 and $biggest"

while [ "$guess" -ne $number ] ; do
  /bin/echo -n "Guess? " ; read guess
  if [ "$guess" -lt $number ] ; then
    echo "... bigger!"
  elif [ "$guess" -gt $number ] ; then
    echo "... smaller!"
  fi
  guesses=$(( $guesses + 1 ))
done

echo "Right!! Guessed $number in $guesses guesses."

exit 0
```

## #16 Working with the Removed File Archive
**Explicacion del codigo:** Se desea buscar en el directorio de archivos eliminados un archivo o directorio especificado por el usuario y si hay mas de un resultado que coincida, muestre una linea de resultados ordenados por marca de tiempo y le permita al usuario cual archivo es el que se desea restaurar.

>Codigo a corregir:

```sh
#!/bin/bash
# unrm--Searches the deleted files archive for the specified file or
# directory. If there is more than one matching result, it shows a list
# of results ordered by timestamp and lets the user specify which one
# to restore.
archivedir="$HOME/.deleted-files"
realrm="$(which rm)"
move="$(which mv)"
dest=$(pwd)
if [ ! -d $archivedir ] ; then
 echo "$0: No deleted files directory: nothing to unrm" >&2
 exit 1
fi
Improving on User Commands 59
cd $archivedir
# If given no arguments, just show a listing of the deleted files.
 if [ $# -eq 0 ] ; then
 echo "Contents of your deleted files archive (sorted by date):"
ls -FC | sed -e 's/\([[:digit:]][[:digit:]]\.\)\{5\}//g' \
 -e 's/^/ /'
 exit 0
fi
# Otherwise, we must have a user-specified pattern to work with.
# Let's see if the pattern matches more than one file or directory
# in the archive. matches="$(ls -d *"$1" 2> /dev/null | wc -l)"
if [ $matches -eq 0 ] ; then
 echo "No match for \"$1\" in the deleted file archive." >&2
 exit 1
fi if [ $matches -gt 1 ] ; then
 echo "More than one file or directory match in the archive:"
 index=1
 for name in $(ls -td *"$1")
 do
 datetime="$(echo $name | cut -c1-14| \
 awk -F. '{ print $5"/"$4" at "$3":"$2":"$1 }')"
 filename="$(echo $name | cut -c16-)"
 if [ -d $name ] ; then
 filecount="$(ls $name | wc -l | sed 's/[^[:digit:]]//g')"
 echo " $index) $filename (contents = ${filecount} items," \
 " deleted = $datetime)"
 else
 size="$(ls -sdk1 $name | awk '{print $1}')"
 echo " $index) $filename (size = ${size}Kb, deleted = $datetime)"
 fi
 index=$(( $index + 1))
 done
 echo ""
 /bin/echo -n "Which version of $1 should I restore ('0' to quit)? [1] : "
 read desired
 if [ ! -z "$(echo $desired | sed 's/[[:digit:]]//g')" ] ; then
 echo "$0: Restore canceled by user: invalid input." >&2
 exit 1
 fi
 if [ ${desired:=1} -ge $index ] ; then
 echo "$0: Restore canceled by user: index value too big." >&2
 exit 1
 fi
60 Chapter 2
 if [ $desired -lt 1 ] ; then
 echo "$0: Restore canceled by user." >&2
 exit 1
 fi
 restore="$(ls -td1 *"$1" | sed -n "${desired}p")"
 if [ -e "$dest/$1" ] ; then
 echo "\"$1\" already exists in this directory. Cannot overwrite." >&2
 exit 1
 fi
 /bin/echo -n "Restoring file \"$1\" ..."
 $move "$restore" "$dest/$1"
 echo "done."
/bin/echo -n "Delete the additional copies of this file? [y] "
 read answer

 if [ ${answer:=y} = "y" ] ; then
 $realrm -rf *"$1"
 echo "Deleted."
 else
 echo "Additional copies retained."
 fi
else
 if [ -e "$dest/$1" ] ; then
 echo "\"$1\" already exists in this directory. Cannot overwrite." >&2
 exit 1
 fi
 restore="$(ls -d *"$1")"
 /bin/echo -n "Restoring file \"$1\" ... "
 $move "$restore" "$dest/$1"
 echo "Done."
fi
exit 0
```

>Codigo corregido:
```sh
#!/bin/bash

archivedir="$HOME/.deleted-files"
realrm="$(which rm)"
move="$(which mv)"

dest=$(pwd)

if [ ! -d $archivedir ] ; then
  echo "$0: No deleted files directory: nothing to unrm" >&2 ; exit 1
fi

cd $archivedir

if [ $# -eq 0 ] ; then
  echo "Contents of your deleted files archive (sorted by date):"
  ls -FC | sed -e 's/\([[:digit:]][[:digit:]]\.\)\{5\}//g' \
    -e 's/^/  /' 
  exit 0
fi

matches="$(ls -d *"$1" 2> /dev/null | wc -l)"

if [ $matches -eq 0 ] ; then
  echo "No match for \"$1\" in the deleted file archive." >&2
  exit 1
fi

if [ $matches -gt 1 ] ; then
  echo "More than one file or directory match in the archive:"
  index=1
  for name in $(ls -td *"$1")
  do
    datetime="$(echo $name | cut -c1-14| \
      awk -F. '{ print $5"/"$4" at "$3":"$2":"$1 }')"
    filename="$(echo $name | cut -c16-)"
    if [ -d $name ] ; then
     filecount="$(ls $name | wc -l | sed 's/[^[:digit:]]//g')"
      echo " $index)   $filename  (contents = ${filecount} items," \ 
           " deleted = $datetime)"
    else
      size="$(ls -sdk1 $name | awk '{print $1}')"
      echo " $index)   $filename  (size = ${size}Kb, deleted = $datetime)"
    fi
    index=$(( $index + 1))
  done
  echo ""
  echo -n "Which version of $1 do you want to restore ('0' to quit)? [1] : "
  read desired
  if [ ! -z "$(echo $desired | sed 's/[[:digit:]]//g')" ] ; then
    echo "$0: Restore canceled by user: invalid input." >&2
    exit 1
  fi

  if [ ${desired:=1} -ge $index ] ; then
    echo "$0: Restore canceled by user: index value too big." >&2
    exit 1
  fi

  if [ $desired -lt 1 ] ; then
    echo "$0: restore canceled by user." >&2 ; exit 1
  fi

  restore="$(ls -td1 *"$1" | sed -n "${desired}p")"
  
  if [ -e "$dest/$1" ] ; then
    echo "\"$1\" already exists in this directory. Cannot overwrite." >&2
    exit 1
  fi

  echo -n "Restoring file \"$1\" ..."
  $move "$restore" "$dest/$1"
  echo "done."

  echo -n "Delete the additional copies of this file? [y] " 
  read answer
  
  if [ ${answer:=y} = "y" ] ; then
    $realrm -rf *"$1"
    echo "deleted."
  else
    echo "additional copies retained."
  fi
else
  if [ -e "$dest/$1" ] ; then
    echo "\"$1\" already exists in this directory. Cannot overwrite." >&2
    exit 1
  fi

  restore="$(ls -d *"$1")"

  echo -n "Restoring file \"$1\" ... " 
  $move "$restore" "$dest/$1"
  echo "done."
fi

exit 0
```

## #19 Locating Files by Filename
**Explicacion de los codigos:** El primer codigo lo que realiza es la creacion de una base de datos de localizacion usando el comando find, cabe aclarar que el usuario tiene que ser root para poder crear dicha base. Mientras que el segundo codigo busca en la base de datos antes creada de localizacion el patron que se le especifico.

>Codigos a corregir:
```sh
#!/bin/bash
# mklocatedb--Builds the locate database using find. User must be root
# to run this script.
locatedb="/var/locate.db"
 if [ "$(whoami)" != "root" ] ; then
 echo "Must be root to run this command." >&2
 exit 1
fi
find / -print > $locatedb
exit 0
```
```sh
#!/bin/sh
# locate--Searches the locate database for the specified pattern
locatedb="/var/locate.db"
exec grep -i "$@" $locatedb
```

>Codigos corregidos:
```sh
#!/bin/bash

locatedb="/tmp/locate.db"

if [ "$(whoami)" != "root" ] ; then
  echo "Must be root to run this command." >&2
  exit 1
fi

find / -print > $locatedb

exit 0
```
```sh
#!/bin/sh

locatedb="/tmp/locate.db"

exec grep -i "$@" $locatedb
```
## #22 A Reminder Utility
**Explicacion de los codigos:** El primer script lo que realiza es permitirle al usuario el poder guardar fragmentos de informacion en un archivo de memoria de su directorio de inicio. El segundo script lo que hace es que muestra el contenido de todo el archivo de memoria cuando no se dan argumentos o muestra los resultados de la búsqueda a través de él utilizando los argumentos como patrón.

>Codigos a corregir:
```sh
#!/bin/bash
# remember--An easy command line-based reminder pad
rememberfile="$HOME/.remember"
if [ $# -eq 0 ] ; then
 # Prompt the user for input and append whatever they write to
 # the rememberfile.
 echo "Enter note, end with ^D: "
cat - >> $rememberfile
else
 # Append any arguments passed to the script on to the .remember file.
 echo "$@" >> $rememberfile
fi
exit 0
```
```sh
#!/bin/bash
# remindme--Searches a data file for matching lines or, if no
# argument is specified, shows the entire contents of the data file
rememberfile="$HOME/.remember"
if [ ! -f $rememberfile ] ; then
 echo "$0: You don't seem to have a .remember file. " >&2
 echo "To remedy this, please use 'remember' to add reminders" >&2
Creating Utilities 81
 exit 1
fi
if [ $# -eq 0 ] ; then
 # Display the whole rememberfile when not given any search criteria.
 more $rememberfile
else
 # Otherwise, search through the file for the given terms, and display
 # the results neatly.
 grep -i -- "$@" $rememberfile | ${PAGER:-more}
fi
exit 0
```

>Codigos corregidos:
```sh
#!/bin/bash

rememberfile="$HOME/.remember"

if [ $# -eq 0 ] ; then

  echo "Enter note, end with ^D: "
  cat - >> $rememberfile
else

  echo "$@" >> $rememberfile
fi

exit 0
```
```sh
#!/bin/bash

rememberfile="$HOME/.remember"

if [ ! -f $rememberfile ] ; then
  echo "$0: You don't seem to have a .remember file." >&2
  echo "To remedy this, please use 'remember' to add reminders" >&2
  exit 1
fi

if [ $# -eq 0 ] ; then

  more $rememberfile
else

  grep -i -- "$@" $rememberfile | ${PAGER:-more}
fi

exit 0
```

## #28 Wrapping Only Long Lines
**Explicacion del codigo:** Un script que fija un tamaño en la longitud de las lineas de flujo de entrada que son mas largas que la longitud especificada que despues devuelve la longitud del contenido de los datos almacenado en la variable varname

>Codigo a corregir:
```sh
#!/bin/bash
# toolong--Feeds the fmt command only those lines in the input stream
# that are longer than the specified length
width=72
if [ ! -r "$1" ] ; then
 echo "Cannot read file $1" >&2
 echo "Usage: $0 filename" >&2
 exit 1
fi
while read input
do
 if [ ${#input} -gt $width ] ; then
 echo "$input" | fmt
 else
 echo "$input"
 fi
  done < $1
exit 0
```

>Codigo corregido:
```sh
#!/bin/bash

width=72

if [ ! -r "$1" ] ; then
  echo "Cannot read file $1" >&2
  echo "Usage: $0 filename" >&2; exit 1
fi

while read input
do
   if [ ${#input} -gt $width ] ; then
     echo "$input" | fmt 
   else
     echo "$input"
   fi
done < $1

exit 0
```

## #29 Displaying a File with Additional Information
**Explicacion del codigo:** Un script que como su nombre lo dice muestra el contenido de un archivo pero esto incluyendo informacion util adicional.

>Codigo a corregir:
```sh
#!/bin/bash
# showfile--Shows the contents of a file, including additional useful info
width=72
for input
do
 lines="$(wc -l < $input | sed 's/ //g')"
 chars="$(wc -c < $input | sed 's/ //g')"
 owner="$(ls -ld $input | awk '{print $3}')"
102 Chapter 4
 echo "-----------------------------------------------------------------"
 echo "File $input ($lines lines, $chars characters, owned by $owner):"
 echo "-----------------------------------------------------------------"
 while read line
 do
 if [ ${#line} -gt $width ] ; then
 echo "$line" | fmt | sed -e '1s/^/ /' -e '2,$s/^/+ /'
 else
 echo " $line"
 fi
 done < $input
 echo "-----------------------------------------------------------------"
done | ${PAGER:more}
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

width=72
for input
do
  echo $input
  lines="$(wc -l < $input | sed 's/ //g')"
  chars="$(wc -c < $input | sed 's/ //g')"
  owner="$(ls -ld $input | awk '{print $3}')"
  echo "-----------------------------------------------------------------"
  echo "File $input ($lines lines, $chars characters, owned by $owner):"
  echo "-----------------------------------------------------------------"
  while read line 
  do
    if [ ${#line} -gt $width ] ; then
      echo "$line" | fmt | sed -e '1s/^/  /' -e '2,$s/^/+ /'
    else
      echo "  $line"
    fi
  done < $input

  echo "-----------------------------------------------------------------"

done | ${PAGER:more}

exit 0
```

## #30 Emulating GNU-Style Flags with quota
**Explicacion del codigo:** Un script para mejorar el funcionamiento del comando quota en el cual funcione con flags de palabra completa a la GNU. Como sabemos el comando quota tiene tres indicadores posibles (-g, -v y -q) pero con este script les permitira ser '--group', '--verbose' y '--quiet' tambien.

>Codigo a corregir:
```sh
#!/bin/bash
# newquota--A frontend to quota that works with full-word flags a la GNU
# quota has three possible flags, -g, -v, and -q, but this script
# allows them to be '--group', '--verbose', and '--quiet' too.
flags=""
realquota="$(which quota)"
while [ $# -gt 0 ]
do
 case $1
 in
 --help) echo "Usage: $0 [--group --verbose --quiet -gvq]" >&2
 exit 1 ;;
 --group) flags="$flags -g"; shift ;;
 --verbose) flags="$flags -v"; shift ;;
 --quiet) flags="$flags -q"; shift ;;
 --) shift; break ;;
 *) break; # Done with 'while' loop!
 esac
done
 exec $realquota $flags "$@"
```
>Codigo corregido:
```sh
#!/bin/bash

flags=""
realquota="$(which quota)"

while [ $# -gt 0 ]
do
  case $1
  in
    --help)  echo "Usage: $0 [--group --verbose --quiet -gvq]" >&2
                       exit 1 ;;
    --group )  flags="$flags -g";       shift ;;
    --verbose)  flags="$flags -v";   shift ;;
    --quiet)  flags="$flags -q";       shift ;;
    --)  shift;           break ;;
    *)  break;         
  esac
done

exec $realquota $flags "$@"
```

## #31 Making sftp Look More Like ftp
**Explicacion del codigo:** Script que permite a usuarios el poder  invocar mysftp exactamente como habrían invocado el programa ftp y se le pedirá que introduzca los campos necesarios.

>Codigo a corregir:
```sh
#!/bin/bash
# mysftp--Makes sftp start up more like ftp
/bin/echo -n "User account: "
read account
if [ -z $account ] ; then
 exit 0; # Changed their mind, presumably
fi
if [ -z "$1" ] ; then
 /bin/echo -n "Remote host: "
 read host
 if [ -z $host ] ; then
 exit 0
 fi
else
 host=$1
fi
# End by switching to sftp. The -C flag enables compression here.
 exec sftp -C $account@$host
```
>Codigo corregido:
```sh
#!/bin/bash


/bin/echo -n "User account: "
read account

if [ -z $account ] ; then
  exit 0;      
fi

if [ -z "$1" ] ; then
  /bin/echo -n "Remote host: "
  read host
  if [ -z $host ] ; then
    exit 0
  fi
else
  host=$1
fi

exec sftp -C $account@$host
```

## #34 Ensuring Maximally Compressed Files
**Explicacion del codigo:** Un script que dado un archivo, intenta comprimirlo con todos las herramientas de compresión y mantiene el archivo comprimido más pequeño, para despues informar el resultado al usuario. En caso de que no se especifique -a, bestcompress se salta archivos comprimidos en el flujo de entrada.

>Codigo a corregir:
```sh
#!/bin/bash
# bestcompress--Given a file, tries compressing it with all the available
# compression tools and keeps the compressed file that's smallest,
# reporting the result to the user. If -a isn't specified, bestcompress
# skips compressed files in the input stream.
Z="compress" gz="gzip" bz="bzip2"
Zout="/tmp/bestcompress.$$.Z"
gzout="/tmp/bestcompress.$$.gz"
bzout="/tmp/bestcompress.$$.bz"
skipcompressed=1
if [ "$1" = "-a" ] ; then
 skipcompressed=0 ; shift
fi
if [ $# -eq 0 ]; then
 echo "Usage: $0 [-a] file or files to optimally compress" >&2
 exit 1
fi
trap "/bin/rm -f $Zout $gzout $bzout" EXIT
for name in "$@"
do
 if [ ! -f "$name" ] ; then
 echo "$0: file $name not found. Skipped." >&2
 continue
 fi
 if [ "$(echo $name | egrep '(\.Z$|\.gz$|\.bz2$)')" != "" ] ; then
 if [ $skipcompressed -eq 1 ] ; then
 echo "Skipped file ${name}: It's already compressed."
 continue
114 Chapter 4
 else
 echo "Warning: Trying to double-compress $name"
 fi
 fi
# Try compressing all three files in parallel.
 $Z < "$name" > $Zout &
 $gz < "$name" > $gzout &
 $bz < "$name" > $bzout &

 wait # Wait until all compressions are done.
# Figure out which compressed best.
 smallest="$(ls -l "$name" $Zout $gzout $bzout | \
 awk '{print $5"="NR}' | sort -n | cut -d= -f2 | head -1)"
 case "$smallest" in
 1 ) echo "No space savings by compressing $name. Left as is."
 ;;
 2 ) echo Best compression is with compress. File renamed ${name}.Z
 mv $Zout "${name}.Z" ; rm -f "$name"
 ;;
 3 ) echo Best compression is with gzip. File renamed ${name}.gz
 mv $gzout "${name}.gz" ; rm -f "$name"
 ;;
 4 ) echo Best compression is with bzip2. File renamed ${name}.bz2
 mv $bzout "${name}.bz2" ; rm -f "$name"
 esac
done
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

Z="compress"     gz="gzip"     bz="bzip2"
Zout="/tmp/bestcompress.$$.Z"
gzout="/tmp/bestcompress.$$.gz"
bzout="/tmp/bestcompress.$$.bz"
skipcompressed=1

if [ "$1" = "-a" ] ; then
  skipcompressed=0  ; shift
fi

if [ $# -eq 0 ]; then
  echo "Usage: $0 [-a] file or files to optimally compress" >&2; exit 1
fi

trap "/bin/rm -f $Zout $gzout $bzout" EXIT

for name in "$@"
do 
  if [ ! -f "$name" ] ; then 
    echo "$0: file $name not found. Skipped." >&2
    continue
  fi

  if [ "$(echo $name | egrep '(\.Z$|\.gz$|\.bz2$)')" != "" ] ; then
    if [ $skipcompressed -eq 1 ] ; then
      echo "Skipped file ${name}: It's already compressed." 
      continue
    else
      echo "Warning: Trying to double-compress $name" 
    fi
  fi

  $Z  < "$name" > $Zout  &
  $gz < "$name" > $gzout &
  $bz < "$name" > $bzout &
  
  wait

  smallest="$(ls -l "$name" $Zout $gzout $bzout | \
     awk '{print $5"="NR}' | sort -n | cut -d= -f2 | head -1)"

  case "$smallest" in
     1 ) echo "No space savings by compressing $name. Left as is."
         ;;
     2 ) echo Best compression is with compress. File renamed ${name}.Z
         mv $Zout "${name}.Z" ; rm -f "$name"
         ;;
     3 ) echo Best compression is with gzip. File renamed ${name}.gz
         mv $gzout "${name}.gz" ; rm -f "$name"
         ;;
     4 ) echo Best compression is with bzip2. File renamed ${name}.bz2
         mv $bzout "${name}.bz2" ; rm -f "$name"
  esac

done

exit 0
```

## #38 Figuring Out Available Disk Space
**Explicacion del codigo:** Un script que resume el espacio disponible en disco y lo presenta en un formato lógico y legible.

>Codigo a corregir:
```sh
#!/bin/bash
# diskspace--Summarizes available disk space and presents it in a logical
# and readable fashion
tempfile="/tmp/available.$$"
126 Chapter 5
trap "rm -f $tempfile" EXIT
cat << 'EOF' > $tempfile
 { sum += $4 }
END { mb = sum / 1024
 gb = mb / 1024
 printf "%.0f MB (%.2fGB) of available disk space\n", mb, gb
 }
EOF
 df -k | awk -f $tempfile
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash


tempfile="/tmp/available.$$"

trap "rm -f $tempfile" EXIT

cat << 'EOF' > $tempfile
    { sum += $4 }
END { mb = sum / 1024
      gb = mb / 1024
      printf "%.0f MB (%.2fGB) of available disk space\n", mb, gb
    }
EOF

df -k | awk -f $tempfile

exit 0
```

## #39 Implementing a Secure locate
**Explicacion de los codigos:** En este script se crea la base de datos de localización pública y central como usuario none, y simultáneamente recorre el directorio de inicio de cada usuario para encontrar aquellos que contienen un archivo .slocatedb. Si lo encuentra, un adicional, privado se creará una versión de la base de datos de localización para ese usuario.

>Codigos a corregir:
```sh
#!/bin/bash
# mkslocatedb--Builds the central, public locate database as user nobody
# and simultaneously steps through each user's home directory to find
# those that contain a .slocatedb file. If found, an additional, private
# version of the locate database will be created for that user.
128 Chapter 5
locatedb="/var/locate.db"
slocatedb=".slocatedb"
if [ "$(id -nu)" != "root" ] ; then
 echo "$0: Error: You must be root to run this command." >&2
 exit 1
fi
if [ "$(grep '^nobody:' /etc/passwd)" = "" ] ; then
 echo "$0: Error: you must have an account for user 'nobody'" >&2
 echo "to create the default slocate database." >&2
 exit 1
fi
cd / # Sidestep post-su pwd permission problems.
# First create or update the public database.
 su -fm nobody -c "find / -print" > $locatedb 2>/dev/null
echo "building default slocate database (user = nobody)"
echo ... result is $(wc -l < $locatedb) lines long.
# Now step through the user accounts on the system to see who has
# a .slocatedb file in their home directory.
for account in $(cut -d: -f1 /etc/passwd)
do
 homedir="$(grep "^${account}:" /etc/passwd | cut -d: -f6)"
 if [ "$homedir" = "/" ] ; then
 continue # Refuse to build one for root dir.
 elif [ -e $homedir/$slocatedb ] ; then
 echo "building slocate database for user $account"
 su -m $account -c "find / -print" > $homedir/$slocatedb \
 2>/dev/null
 chmod 600 $homedir/$slocatedb
 chown $account $homedir/$slocatedb
 echo ... result is $(wc -l < $homedir/$slocatedb) lines long.
 fi
done
exit 0
```
```sh
#!/bin/bash
# slocate--Tries to search the user's own secure locatedb database for the
# specified pattern. If the pattern doesn't match, it means no database
# exists, so it outputs a warning and creates one. If personal .slocatedb
# is empty, it uses system database instead.
locatedb="/var/locate.db"
slocatedb="$HOME/.slocatedb"
System Administration: Managing Users 129
if [ ! -e $slocatedb -o "$1" = "--explain" ] ; then
 cat << "EOF" >&2
Warning: Secure locate keeps a private database for each user, and your
database hasn't yet been created. Until it is (probably late tonight),
I'll just use the public locate database, which will show you all
publicly accessible matches rather than those explicitly available to
account ${USER:-$LOGNAME}.
EOF
 if [ "$1" = "--explain" ] ; then
 exit 0
 fi
 # Before we go, create a .slocatedb file so that cron will fill it
 # the next time the mkslocatedb script is run.
 touch $slocatedb # mkslocatedb will build it next time through.
 chmod 600 $slocatedb # Start on the right foot with permissions.
elif [ -s $slocatedb ] ; then
 locatedb=$slocatedb
else
 echo "Warning: using public database. Use \"$0 --explain\" for details." >&2
fi
if [ -z "$1" ] ; then
 echo "Usage: $0 pattern" >&2
 exit 1
fi
exec grep -i "$1" $locatedb
```
>Codigos corregidos:
```sh
#!/bin/bash

locatedb="/var/locate.db"
slocatedb=".slocatedb"

if [ "$(id -nu)" != "root" ] ; then
  echo "$0: Error: You must be root to run this command." >&2
  exit 1
fi

if [ "$(grep '^nobody:' /etc/passwd)" = "" ] ; then
  echo "$0: Error: you must have an account for user 'nobody'" >&2
  echo "to create the default slocate database." >&2; exit 1
fi

cd /            

su -fm nobody -c "find / -print" > $locatedb 2>/dev/null
echo "building default slocate database (user = nobody)"
echo ... result is $(wc -l < $locatedb) lines long.

for account in $(cut -d: -f1 /etc/passwd)
do
  homedir="$(grep "^${account}:" /etc/passwd | cut -d: -f6)"

  if [ "$homedir" = "/" ] ; then
    continue
  elif [ -e $homedir/$slocatedb ] ; then
    echo "building slocate database for user $account"
    su -m $account -c "find / -print" > $homedir/$slocatedb \
     2>/dev/null
    chmod 600 $homedir/$slocatedb
    chown $account $homedir/$slocatedb
    echo ... result is $(wc -l < $homedir/$slocatedb) lines long.
  fi
done

exit 0
```
```sh
#!/bin/bash

locatedb="/var/locate.db"
slocatedb="$HOME/.slocatedb"

if [ ! -e $slocatedb -o "$1" = "--explain" ] ; then
  cat << "EOF" >&2
Warning: Secure locate keeps a private database for each user, and your
database hasn't yet been created. Until it is (probably late tonight),
I'll just use the public locate database, which will show you all
publicly accessible matches rather than those explicitly available to
account ${USER:-$LOGNAME}.
EOF
  if [ "$1" = "--explain" ] ; then
    exit 0
  fi

  touch $slocatedb   
  chmod 600 $slocatedb 

elif [ -s $slocatedb ] ; then
  locatedb=$slocatedb
else
  echo "Warning: using public database. Use \"$0 --explain\" for details." >&2
fi

if [ -z "$1" ] ; then
  echo "Usage: $0 pattern" >&2; exit 1
fi

exec grep -i "$1" $locatedb
```

## #41 Suspending a User Account
**Explicacion del codigo:** Un script que suspende a un usuario del sistema mandando un mensaje de que sera suspendido si este esta activo para despues a traves del script cambiar su contraseña para que este no pueda ingresar.


>Codigo a corregir:
```sh
#!/bin/bash
# suspenduser--Suspends a user account for the indefinite future
homedir="/home" # Home directory for users
secs=10 # Seconds before user is logged out
if [ -z $1 ] ; then
 echo "Usage: $0 account" >&2
 exit 1
elif [ "$(id -un)" != "root" ] ; then
 echo "Error. You must be 'root' to run this command." >&2
 exit 1
fi
echo "Please change the password for account $1 to something new."
passwd $1
# Now let's see if they're logged in and, if so, boot 'em.
if who|grep "$1" > /dev/null ; then
 for tty in $(who | grep $1 | awk '{print $2}'); do
 cat << "EOF" > /dev/$tty
******************************************************************************
URGENT NOTICE FROM THE ADMINISTRATOR:
This account is being suspended, and you are going to be logged out
in $secs seconds. Please immediately shut down any processes you
have running and log out.
If you have any questions, please contact your supervisor or
John Doe, Director of Information Technology.
******************************************************************************
EOF
 done
 echo "(Warned $1, now sleeping $secs seconds)"
 sleep $secs
System Administration: Managing Users 135
 jobs=$(ps -u $1 | cut -d\ -f1)
X kill -s HUP $jobs # Send hangup sig to their processes.
 sleep 1 # Give it a second...
Y kill -s KILL $jobs > /dev/null 2>1 # and kill anything left.
 echo "$1 was logged in. Just logged them out."
fi
# Finally, let's close off their home directory from prying eyes.
chmod 000 $homedir/$1
echo "Account $1 has been suspended."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

homedir="/home"        
secs=10             

if [ -z $1 ] ; then
  echo "Usage: $0 account" >&2 ; exit 1
elif [ "$(id -un)" != "root" ] ; then
  echo "Error. You must be 'root' to run this command." >&2; exit 1
fi

echo "Please change account $1 password to something new."
passwd $1

if who|grep "$1" > /dev/null ; then

  for tty in $(who | grep $1 | awk '{print $2}'); do

    cat << "EOF" > /dev/$tty
*************************************************************
URGENT NOTICE FROM THE ADMINISTRATOR:
This account is being suspended, and you are going to be logged out 
in $secs seconds. Please immediately shut down any processes you 
have running and log out.
If you have any questions, please contact your supervisor or 
John Doe, Director of Information Technology.
*************************************************************
EOF
  done

  echo "(Warned $1, now sleeping $secs seconds)"

  sleep $secs

  jobs=$(ps -u $1 | cut -d\  -f1)

  kill -s HUP $jobs
  sleep 1        
  kill -s KILL $jobs > /dev/null 2>1 

  echo "$1 was logged in. Just logged them out."
fi

chmod 000 $homedir/$1

echo "Account $1 has been suspended."

exit 0
```

## #42 Deleting a User Account
**Explicacion del codigo:** En el siguiente script a diferencia del anterior lo que se desea es eliminar al usuario y toda su informacion del sistema sin dejar rastro alguno.

>Codigo a corregir:
```sh
#!/bin/bash
# deleteuser--Deletes a user account without a trace.
# Not for use with OS X.
homedir="/home"
pwfile="/etc/passwd"
shadow="/etc/shadow"
newpwfile="/etc/passwd.new"
newshadow="/etc/shadow.new"
suspend="$(which suspenduser)"
locker="/etc/passwd.lock"
if [ -z $1 ] ; then
 echo "Usage: $0 account" >&2
 exit 1
elif [ "$(whoami)" != "root" ] ; then
 echo "Error: you must be 'root' to run this command.">&2
 exit 1
fi
System Administration: Managing Users 137
$suspend $1 # Suspend their account while we do the dirty work.
uid="$(grep -E "^${1}:" $pwfile | cut -d: -f3)"
if [ -z $uid ] ; then
 echo "Error: no account $1 found in $pwfile" >&2
 exit 1
fi
# Remove the user from the password and shadow files.
grep -vE "^${1}:" $pwfile > $newpwfile
grep -vE "^${1}:" $shadow > $newshadow
lockcmd="$(which lockfile)" # Find lockfile app in the path.
 if [ ! -z $lockcmd ] ; then # Let's use the system lockfile.
 eval $lockcmd -r 15 $locker
else # Ulp, let's do it ourselves.
 while [ -e $locker ] ; do
 echo "waiting for the password file" ; sleep 1
 done
 touch $locker # Create a file-based lock.
fi
mv $newpwfile $pwfile
mv $newshadow $shadow
 rm -f $locker # Click! Unlocked again.
chmod 644 $pwfile
chmod 400 $shadow
# Now remove home directory and list anything left.
rm -rf $homedir/$1
echo "Files still left to remove (if any):"
find / -uid $uid -print 2>/dev/null | sed 's/^/ /'
echo ""
echo "Account $1 (uid $uid) has been deleted, and their home directory "
echo "($homedir/$1) has been removed."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

homedir="/home"
pwfile="/etc/passwd"            
shadow="/etc/shadow"
newpwfile="/etc/passwd.new"     
newshadow="/etc/shadow.new"
locker="/etc/passwd.lock"

if [ -z $1 ] ; then
  echo "Usage: $0 account" >&2; exit 1
elif [ "$(whoami)" != "root" ] ; then
  echo "Error: you must be 'root' to run this command.">&2; exit 1
fi

suspenduser $1    

uid="$(grep -E "^${1}:" $pwfile | cut -d: -f3)"

if [ -z $uid ] ; then
 echo "Error: no account $1 found in $pwfile" >&2; exit 1
fi


grep -vE "^${1}:" $pwfile > $newpwfile
grep -vE "^${1}:" $shadow > $newshadow

lockcmd="$(which lockfile)"           
if [ ! -z $lockcmd ] ; then            
  eval $lockcmd -r 15 $locker 
else                                  
  while [ -e $locker ] ; do
    echo "waiting for the password file" ; sleep 1
  done
  touch $locker                     

mv $newpwfile $pwfile 
mv $newshadow $shadow 
rm -f $locker                          

chmod 644 $pwfile
chmod 400 $shadow

rm -rf $homedir/$1

echo "Files still left to remove (if any):"
find / -uid $uid -print 2>/dev/null | sed 's/^/  /'

echo ""
echo "Account $1 (uid $uid) has been deleted, and their home directory "
echo "($homedir/$1) has been removed."

exit 0
```

## #43 Validating the User Environment
**Explicacion del codigo:** Un script que comprueba para asegurarse de que la RUTA contiene solo directorios válidos para despues verificar cada una de las configuraciones clave de la aplicación de ayuda para asegurarse de que estén ya sea indicando un archivo completamente calificado que existe o especificando un archivo binario

>Codigo a corregir:
```sh
#!/bin/bash
# validator--Ensures that the PATH contains only valid directories
# and then checks that all environment variables are valid.
# Looks at SHELL, HOME, PATH, EDITOR, MAIL, and PAGER.
errors=0
 source library.sh # This contains Script #1, the in_path() function.
 validate()
{
 varname=$1
 varvalue=$2

 if [ ! -z $varvalue ] ; then
 if [ "${varvalue%${varvalue#?}}" = "/" ] ; then
 if [ ! -x $varvalue ] ; then
 echo "** $varname set to $varvalue, but I cannot find executable."
 (( errors++ ))
 fi
 else
 if in_path $varvalue $PATH ; then
 echo "** $varname set to $varvalue, but I cannot find it in PATH."
 errors=$(( $errors + 1 ))
 fi
 fi
 fi
}
# BEGIN MAIN SCRIPT
# =================
 if [ ! -x ${SHELL:?"Cannot proceed without SHELL being defined."} ] ; then
 echo "** SHELL set to $SHELL, but I cannot find that executable."
 errors=$(( $errors + 1 ))
fi
140 Chapter 5
if [ ! -d ${HOME:?"You need to have your HOME set to your home directory"} ]
then
 echo "** HOME set to $HOME, but it's not a directory."
 errors=$(( $errors + 1 ))
fi
# Our first interesting test: Are all the paths in PATH valid?
 oldIFS=$IFS; IFS=":" # IFS is the field separator. We'll change to ':'.
 for directory in $PATH
do
 if [ ! -d $directory ] ; then
 echo "** PATH contains invalid directory $directory."
 errors=$(( $errors + 1 ))
 fi
done
IFS=$oldIFS # Restore value for rest of script.
# The following variables should each be a fully qualified path,
# but they may be either undefined or a progname. Add additional
# variables as necessary for your site and user community.
validate "EDITOR" $EDITOR
validate "MAILER" $MAILER
validate "PAGER" $PAGER
# And, finally, a different ending depending on whether errors > 0
if [ $errors -gt 0 ] ; then
 echo "Errors encountered. Please notify sysadmin for help."
else
 echo "Your environment checks out fine."
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

errors=0

source ../1/library.sh 

validate()
{
  varname=$1    
  varvalue=$2
  
  if [ ! -z $varvalue ] ; then
    if [ "${varvalue%${varvalue#?}}" = "/" ] ; then
      if [ ! -x $varvalue ] ; then
        echo "** $varname set to $varvalue, but I cannot find executable."
        (( errors++ ))
      fi
    else
      if in_path $varvalue $PATH ; then 
        echo "** $varname set to $varvalue, but I cannot find it in PATH."
        errors=$(( $errors + 1 ))
      fi
    fi 
  fi
}

if [ ! -x ${SHELL:?"Cannot proceed without SHELL being defined."} ] ; then
  echo "** SHELL set to $SHELL, but I cannot find that executable."
  errors=$(( $errors + 1 ))
fi

if [ ! -d ${HOME:?"You need to have your HOME set to your home directory"} ]
then
  echo "** HOME set to $HOME, but it's not a directory."
  errors=$(( $errors + 1 ))
fi

oldIFS=$IFS; IFS=":" 

for directory in $PATH
do
  if [ ! -d $directory ] ; then
      echo "** PATH contains invalid directory $directory"
      errors=$(( $errors + 1 ))
  fi
done

IFS=$oldIFS       

validate "EDITOR" $EDITOR
validate "MAILER" $MAILER
validate "PAGER"  $PAGER

if [ $errors -gt 0 ] ; then
  echo "Errors encountered. Please notify sysadmin for help."
else
  echo "Your environment checks out fine."
fi

exit 0
```

## #44 Cleaning Up After Guests Leave
**Explicacion del codigo:** Un script que limpia la cuenta de invitado durante el proceso de cierre de sesion. Borra cualquier archivo o subdirectorio recién creado, elimina todos los archivos de puntos y reconstruye los archivos de la cuenta oficial, cuyas copias se almacenan en un archivo de solo lectura escondido en el directorio .template de la cuenta de invitado

>Codigo a corregir:
```sh
#!/bin/bash
# fixguest--Cleans up the guest account during the logout process
# Don't trust environment variables: reference read-only sources.
iam=$(id -un)
myhome="$(grep "^${iam}:" /etc/passwd | cut -d: -f6)"
# *** Do NOT run this script on a regular user account!
if [ "$iam" != "guest" ] ; then
 echo "Error: you really don't want to run fixguest on this account." >&2
 exit 1
fi
if [ ! -d $myhome/..template ] ; then
 echo "$0: no template directory found for rebuilding." >&2
 exit 1
fi
# Remove all files and directories in the home account.
cd $myhome
rm -rf * $(find . -name ".[a-zA-Z0-9]*" -print)
# Now the only thing present should be the ..template directory.
cp -Rp ..template/* .
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

iam=$(id -un)
myhome="$(grep "^${iam}:" /etc/passwd | cut -d: -f6)"

if [ "$iam" != "guest" ] ; then
  echo "Error: you really don't want to run fixguest on this account." >&2
  exit 1
fi

if [ ! -d $myhome/..template ] ; then
  echo "$0: no template directory found for rebuilding." >&2
  exit 1
fi

cd $myhome

rm -rf * $(find . -name ".[a-zA-Z0-9]*" -print)

cp -Rp ..template/* .
exit 0
```

## #46 Setting the System Date
**Explicacion del codigo:** Un script que facilita el comando de cambio de fecha del sistema.Para que las cosas sean mas faciles dde usar sta función solicita una fecha específica, despues la guarda en un valor para despues mostrar el valor predeterminado en [] basado en la fecha y hora actuales.

>Codigo a corregir:
```sh
#!/bin/bash
# setdate--Friendly frontend to the date command
# Date wants: [[[[[cc]yy]mm]dd]hh]mm[.ss]
# To make things user-friendly, this function prompts for a specific date
# value, displaying the default in [] based on the current date and time.
. library.sh # Source our library of bash functions to get echon().
 askvalue()
{
 # $1 = field name, $2 = default value, $3 = max value,
 # $4 = required char/digit length
 echon "$1 [$2] : "
 read answer
System Administration: System Maintenance 149
 if [ ${answer:=$2} -gt $3 ] ; then
 echo "$0: $1 $answer is invalid"
 exit 0
 elif [ "$(( $(echo $answer | wc -c) - 1 ))" -lt $4 ] ; then
 echo "$0: $1 $answer is too short: please specify $4 digits"
 exit 0
 fi
 eval $1=$answer # Reload the given variable with the specified value.
}
 eval $(date "+nyear=%Y nmon=%m nday=%d nhr=%H nmin=%M")
askvalue year $nyear 3000 4
askvalue month $nmon 12 2
askvalue day $nday 31 2
askvalue hour $nhr 24 2
askvalue minute $nmin 59 2
squished="$year$month$day$hour$minute"
# Or, if you're running a Linux system:
 # squished="$month$day$hour$minute$year"
# Yes, Linux and OS X/BSD systems use different formats. Helpful, eh?
echo "Setting date to $squished. You might need to enter your sudo password:"
sudo date $squished
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

. ../1/library.sh 

askvalue()
{

  echon "$1 [$2] : "
  read answer
  if [ ${answer:=$2} -gt $3 ] ; then
    echo "$0: $1 $answer is invalid"; exit 0
  elif [ "$(( $(echo $answer | wc -c) - 1 ))" -lt $4 ] ; then
    echo "$0: $1 $answer is too short: please specify $4 digits"; exit 0
  fi
  eval $1=$answer  
}

eval $(date "+nyear=%Y nmon=%m nday=%d nhr=%H nmin=%M")

askvalue year $nyear 3000 4
askvalue month $nmon 12 2
askvalue day $nday 31 2
askvalue hour $nhr 24 2
askvalue minute $nmin 59 2

squished="$month$day$hour$minute$year"

echo "Setting date to $squished. You might need to enter your sudo password:"
sudo date $squished

exit 0
```

## #48 Validating User crontab Entries
**Explicacion del codigo:** Un scipt que comprueba un archivo crontab para asegurarse de que es formateado correctamente. Despues espera la notación cron estándar de min hr dom lun dow CMD, donde min es 0-59, hr es 0-23, dom es 1-31, mon es 1-12 (o nombres) y dow es 0-7 (o nombres). Los campos pueden ser rangos (ae) o listas separados por comas (a,c,z) o un asterisco. Tenga en cuenta que el paso notación de valor de Vixie cron (por ejemplo, 2-6/2) no es compatible con este script en su versión actual.

>Codigo a corregir:
```sh
#!/bin/bash
# verifycron--Checks a crontab file to ensure that it's formatted properly.
# Expects standard cron notation of min hr dom mon dow CMD, where min is
# 0-59, hr is 0-23, dom is 1-31, mon is 1-12 (or names), and dow is 0-7
# (or names). Fields can be ranges (a-e) or lists separated by commas
# (a,c,z) or an asterisk. Note that the step value notation of Vixie cron
# (e.g., 2-6/2) is not supported by this script in its current version.
validNum()
{
 # Return 0 if the number given is a valid integer and 1 if not.
 # Specify both number and maxvalue as args to the function.
 num=$1 max=$2
System Administration: System Maintenance 155
 # Asterisk values in fields are rewritten as "X" for simplicity,
 # so any number in the form "X" is de facto valid.
 if [ "$num" = "X" ] ; then
 return 0
 elif [ ! -z $(echo $num | sed 's/[[:digit:]]//g') ] ; then
 # Stripped out all the digits, and the remainder isn't empty? No good.
 return 1
 elif [ $num -gt $max ] ; then
 # Number is bigger than the maximum value allowed.
 return 1
 else
 return 0
 fi
}
validDay()
{
 # Return 0 if the value passed to this function is a valid day name;
 # 1 otherwise.
 case $(echo $1 | tr '[:upper:]' '[:lower:]') in
 sun*|mon*|tue*|wed*|thu*|fri*|sat*) return 0 ;;
 X) return 0 ;; # Special case, it's a rewritten "*"
 *) return 1
 esac
}
validMon()
{
 # This function returns 0 if given a valid month name; 1 otherwise.
 case $(echo $1 | tr '[:upper:]' '[:lower:]') in
 jan*|feb*|mar*|apr*|may|jun*|jul*|aug*) return 0 ;;
 sep*|oct*|nov*|dec*) return 0 ;;
 X) return 0 ;; # Special case, it's a rewritten "*"
 *) return 1 ;;
 esac
}
 fixvars()
{
 # Translate all '*' into 'X' to bypass shell expansion hassles.
 # Save original input as "sourceline" for error messages.
 sourceline="$min $hour $dom $mon $dow $command"
 min=$(echo "$min" | tr '*' 'X') # Minute
 hour=$(echo "$hour" | tr '*' 'X') # Hour
 dom=$(echo "$dom" | tr '*' 'X') # Day of month
 mon=$(echo "$mon" | tr '*' 'X') # Month
 dow=$(echo "$dow" | tr '*' 'X') # Day of week
}
156 Chapter 6
if [ $# -ne 1 ] || [ ! -r $1 ] ; then
 # If no crontab filename is given or if it's not readable by the script, fail.
 echo "Usage: $0 usercrontabfile" >&2
 exit 1
fi
lines=0 entries=0 totalerrors=0
# Go through the crontab file line by line, checking each one.
while read min hour dom mon dow command
do
 lines="$(( $lines + 1 ))"
 errors=0

 if [ -z "$min" -o "${min%${min#?}}" = "#" ] ; then
 # If it's a blank line or the first character of the line is "#", skip it.
 continue # Nothing to check
 fi
 ((entries++))
 fixvars
 # At this point, all the fields in the current line are split out into
 # separate variables, with all asterisks replaced by "X" for convenience,
 # so let's check the validity of input fields...
 # Minute check
 for minslice in $(echo "$min" | sed 's/[,-]/ /g') ; do
 if ! validNum $minslice 60 ; then
 echo "Line ${lines}: Invalid minute value \"$minslice\""
 errors=1
 fi
 done
 # Hour check

 for hrslice in $(echo "$hour" | sed 's/[,-]/ /g') ; do
 if ! validNum $hrslice 24 ; then
 echo "Line ${lines}: Invalid hour value \"$hrslice\""
 errors=1
 fi
 done
 # Day of month check
 for domslice in $(echo $dom | sed 's/[,-]/ /g') ; do
 if ! validNum $domslice 31 ; then
 echo "Line ${lines}: Invalid day of month value \"$domslice\""
 errors=1
 fi
 done
System Administration: System Maintenance 157
 # Month check: Has to check for numeric values and names both.
 # Remember that a conditional like "if ! cond" means that it's
 # testing whether the specified condition is FALSE, not true.
 for monslice in $(echo "$mon" | sed 's/[,-]/ /g') ; do
 if ! validNum $monslice 12 ; then
 if ! validMon "$monslice" ; then
 echo "Line ${lines}: Invalid month value \"$monslice\""
 errors=1
 fi
 fi
 done
 # Day of week check: Again, name or number is possible.
 for dowslice in $(echo "$dow" | sed 's/[,-]/ /g') ; do
 if ! validNum $dowslice 7 ; then
 if ! validDay $dowslice ; then
 echo "Line ${lines}: Invalid day of week value \"$dowslice\""
 errors=1
 fi
 fi
 done
 if [ $errors -gt 0 ] ; then
 echo ">>>> ${lines}: $sourceline"
 echo ""
 totalerrors="$(( $totalerrors + 1 ))"
 fi
done < $1 # read the crontab passed as an argument to the script
# Notice that it's here, at the very end of the while loop, that we
# redirect the input so that the user-specified filename can be
# examined by the script!
echo "Done. Found $totalerrors errors in $entries crontab entries."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

validNum()
{

  num=$1   max=$2

  if [ "$num" = "X" ] ; then
    return 0
  elif [ ! -z $(echo $num | sed 's/[[:digit:]]//g') ] ; then
    return 1
  elif [ $num -gt $max ] ; then
    return 1
  else
    return 0
  fi
}

validDay()
{

  case $(echo $1 | tr '[:upper:]' '[:lower:]') in
    sun*|mon*|tue*|wed*|thu*|fri*|sat*) return 0 ;;
    X) return 0 ;;         
    *) return 1
  esac
}

validMon()
{

   case $(echo $1 | tr '[:upper:]' '[:lower:]') in 
     jan*|feb*|mar*|apr*|may|jun*|jul*|aug*) return 0           ;;
     sep*|oct*|nov*|dec*)                    return 0           ;;
     X) return 0 ;; 
     *) return 1        ;;
   esac
}

fixvars()
{

  sourceline="$min $hour $dom $mon $dow $command"
   min=$(echo "$min" | tr '*' 'X')    
  hour=$(echo "$hour" | tr '*' 'X')    
   dom=$(echo "$dom" | tr '*' 'X')     
   mon=$(echo "$mon" | tr '*' 'X')      
   dow=$(echo "$dow" | tr '*' 'X')      
}

if [ $# -ne 1 ] || [ ! -r $1 ] ; then
  echo "Usage: $0 usercrontabfile" >&2; exit 1
fi

lines=0  entries=0  totalerrors=0

while read min hour dom mon dow command
do
  lines="$(( $lines + 1 ))" 
  errors=0
  
  if [ -z "$min" -o "${min%${min#?}}" = "#" ] ; then
    continue   
  fi


  ((entries++))

  fixvars

  for minslice in $(echo "$min" | sed 's/[,-]/ /g') ; do
    if ! validNum $minslice 60 ; then
      echo "Line ${lines}: Invalid minute value \"$minslice\""
      errors=1
    fi
  done
  
  for hrslice in $(echo "$hour" | sed 's/[,-]/ /g') ; do
    if ! validNum $hrslice 24 ; then
      echo "Line ${lines}: Invalid hour value \"$hrslice\"" 
      errors=1
    fi
  done

  for domslice in $(echo $dom | sed 's/[,-]/ /g') ; do
    if ! validNum $domslice 31 ; then
      echo "Line ${lines}: Invalid day of month value \"$domslice\""
      errors=1
    fi
  done

  for monslice in $(echo "$mon" | sed 's/[,-]/ /g') ; do
    if ! validNum $monslice 12 ; then
      if ! validMon "$monslice" ; then
        echo "Line ${lines}: Invalid month value \"$monslice\""
        errors=1
      fi
    fi
  done

  for dowslice in $(echo "$dow" | sed 's/[,-]/ /g') ; do
    if ! validNum $dowslice 7 ; then
      if ! validDay $dowslice ; then
        echo "Line ${lines}: Invalid day of week value \"$dowslice\""
        errors=1
      fi
    fi
  done

  if [ $errors -gt 0 ] ; then
    echo ">>>> ${lines}: $sourceline"
    echo ""
    totalerrors="$(( $totalerrors + 1 ))"
  fi
done < $1

echo "Done. Found $totalerrors errors in $entries crontab entries."

exit 0
```

## #49 Ensuring that System cron Jobs Are Run
**Explicacion del codigo:** Un script que ejecuta los trabajos cron diarios, semanales y mensuales del sistema en un sistema que es probable que se apague durante la hora habitual del día cuando de lo contrario, los trabajos cron del sistema estarían programados para ejecutarse.

>Codigo a corregir:
```sh
#!/bin/bash
# docron--Runs the daily, weekly, and monthly system cron jobs on a system
# that's likely to be shut down during the usual time of day when the system
# cron jobs would otherwise be scheduled to run.
rootcron="/etc/crontab" # This is going to vary significantly based on
 # which version of Unix or Linux you've got.
if [ $# -ne 1 ] ; then
 echo "Usage: $0 [daily|weekly|monthly]" >&2
 exit 1
fi
# If this script isn't being run by the administrator, fail out.
# In earlier scripts, you saw USER and LOGNAME being tested, but in
# this situation, we'll check the user ID value directly. Root = 0.
if [ "$(id -u)" -ne 0 ] ; then
 # Or you can use $(whoami) != "root" here, as needed.
 echo "$0: Command must be run as 'root'" >&2
 exit 1
fi
160 Chapter 6
# We assume that the root cron has entries for 'daily', 'weekly', and
# 'monthly' jobs. If we can't find a match for the one specified, well,
# that's an error. But first, we'll try to get the command if there is
# a match (which is what we expect).
 job="$(awk "NF > 6 && /$1/ { for (i=7;i<=NF;i++) print \$i }" $rootcron)"
if [ -z "$job" ] ; then # No job? Weird. Okay, that's an error.
 echo "$0: Error: no $1 job found in $rootcron" >&2
 exit 1
fi
SHELL=$(which sh) # To be consistent with cron's default
 eval $job # We'll exit once the job is finished.
```
>Codigo corregido:
```sh
#!/bin/bash

rootcron="/etc/crontab"   

if [ $# -ne 1 ] ; then
  echo "Usage: $0 [daily|weekly|monthly]" >&2
  exit 1
fi

if [ "$(id -u)" -ne 0 ] ; then
  echo "$0: Command must be run as 'root'" >&2
  exit 1
fi

job="$(awk "NF > 6 && /$1/ { for (i=7;i<=NF;i++) print \$i }" $rootcron)"

if [ -z "$job" ] ; then  
  echo "$0: Error: no $1 job found in $rootcron" >&2
  exit 1
fi

SHELL='which sh'         

eval $job             
```

## #51 Managing Backups
**Explicacion del codigo:** Un script que crea una copia de seguridad completa o incremental de un conjunto de directorios definidos en el sistema. Por defecto, la salida. El archivo se comprime y se guarda en /tmp con un nombre de archivo con marca de tiempo. De lo contrario, especifique un dispositivo de salida (otro disco, un dispositivo extraíble dispositivo de almacenamiento, o cualquier otra cosa que haga flotar su bote).

>Codigo a corregir:
```sh
#!/bin/bash
# backup--Creates either a full or incremental backup of a set of defined
# directories on the system. By default, the output file is compressed and
# saved in /tmp with a timestamped filename. Otherwise, specify an output
# device (another disk, a removable storage device, or whatever else floats
# your boat).
System Administration: System Maintenance 167
compress="bzip2" # Change to your favorite compression app.
 inclist="/tmp/backup.inclist.$(date +%d%m%y)"
 output="/tmp/backup.$(date +%d%m%y).bz2"
 tsfile="$HOME/.backup.timestamp"
 btype="incremental" # Default to an incremental backup.
 noinc=0 # And here's an update of the timestamp.
trap "/bin/rm -f $inclist" EXIT
usageQuit()
{
 cat << "EOF" >&2
Usage: $0 [-o output] [-i|-f] [-n]
 -o lets you specify an alternative backup file/device,
 -i is an incremental, -f is a full backup, and -n prevents
 updating the timestamp when an incremental backup is done.
EOF
 exit 1
}
########## Main code section begins here ###########
while getopts "o:ifn" arg; do
 case "$opt" in
 o ) output="$OPTARG"; ;; # getopts automatically manages OPTARG.
 i ) btype="incremental"; ;;
 f ) btype="full"; ;;
 n ) noinc=1; ;;
 ? ) usageQuit ;;
 esac
done
shift $(( $OPTIND - 1 ))
echo "Doing $btype backup, saving output to $output"
timestamp="$(date +'%m%d%I%M')" # Grab month, day, hour, minute from date.
 # Curious about date formats? "man strftime"
if [ "$btype" = "incremental" ] ; then
 if [ ! -f $tsfile ] ; then
 echo "Error: can't do an incremental backup: no timestamp file" >&2
 exit 1
 fi
 find $HOME -depth -type f -newer $tsfile -user ${USER:-LOGNAME} | \
 pax -w -x tar | $compress > $output
 failure="$?"
else
 find $HOME -depth -type f -user ${USER:-LOGNAME} | \
 pax -w -x tar | $compress > $output
 failure="$?"
fi
168 Chapter 6
if [ "$noinc" = "0" -a "$failure" = "0" ] ; then
 touch -t $timestamp $tsfile
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

compress="bzip2"
inclist="/tmp/backup.inclist.$(date +%d%m%y)"
 output="/tmp/backup.$(date +%d%m%y).bz2"
 tsfile="$HOME/.backup.timestamp"
  btype="incremental"  
  noinc=0              

trap "/bin/rm -f $inclist" EXIT

usageQuit()
{
  cat << "EOF" >&2
Usage: $0 [-o output] [-i|-f] [-n]
  -o lets you specify an alternative backup file/device,
  -i is an incremental, -f is a full backup, and -n prevents
  updating the timestamp when an incremental backup is done.
EOF
  exit 1
}

while getopts "o:ifn" arg; do
  case "$opt" in
    o ) output="$OPTARG";       ;;   
    i ) btype="incremental";    ;;
    f ) btype="full";           ;;
    n ) noinc=1;                ;;
    ? ) usageQuit               ;;
  esac
done

shift $(( $OPTIND - 1 ))

echo "Doing $btype backup, saving output to $output"

timestamp="$(date +'%m%d%I%M')"  


if [ "$btype" = "incremental" ] ; then 
  if [ ! -f $tsfile ] ; then
    echo "Error: can't do an incremental backup: no timestamp file" >&2
    exit 1
  fi
  find $HOME -depth -type f -newer $tsfile -user ${USER:-LOGNAME} | \
  pax -w -x tar | $compress > $output
  failure="$?"
else
  find $HOME -depth -type f -user ${USER:-LOGNAME} | \
  pax -w -x tar | $compress > $output
  failure="$?"
fi

if [ "$noinc" = "0" -a "$failure" = "0" ] ; then
  touch -t $timestamp $tsfile
fi
exit 0
```

## #52 Backing Up Directories
**Explicacion del codigo:** Un script que permite a los usuarios crear un archivo tar comprimido de un directorio especificado para archivar o compartir.

>Codigo a corregir:
```sh
#!/bin/bash
# archivedir--Creates a compressed archive of the specified directory
maxarchivedir=10 # Size, in blocks, of big directory.
compress=gzip # Change to your favorite compress app.
progname=$(basename $0) # Nicer output format for error messages.
if [ $# -eq 0 ] ; then # No args? That's a problem.
 echo "Usage: $progname directory" >&2
 exit 1
fi
if [ ! -d $1 ] ; then
 echo "${progname}: can't find directory $1 to archive." >&2
 exit 1
fi
if [ "$(basename $1)" != "$1" -o "$1" = "." ] ; then
 echo "${progname}: You must specify a subdirectory" >&2
 exit 1
fi if [ ! -w . ] ; then
 echo "${progname}: cannot write archive file to current directory." >&2
 exit 1
fi
170 Chapter 6
# Is the resultant archive going to be dangerously big? Let's check...
dirsize="$(du -s $1 | awk '{print $1}')"
if [ $dirsize -gt $maxarchivedir ] ; then
 /bin/echo -n "Warning: directory $1 is $dirsize blocks. Proceed? [n] "
 read answer
 answer="$(echo $answer | tr '[:upper:]' '[:lower:]' | cut -c1)"
 if [ "$answer" != "y" ] ; then
 echo "${progname}: archive of directory $1 canceled." >&2
 exit 0
 fi
fi
archivename="$1.tgz"
if tar cf - $1 | $compress > $archivename ; then
 echo "Directory $1 archived as $archivename"
else
 echo "Warning: tar encountered errors archiving $1"
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

maxarchivedir=10         
compress=gzip              
progname=$(basename $0)    

if [ $# -eq 0 ] ; then     
  echo "Usage: $progname directory" >&2 ;exit 1
fi

if [ ! -d $1 ] ; then
  echo "${progname}: can't find directory $1 to archive." >&2; exit 1
fi

if [ "$(basename $1)" != "$1" -o "$1" = "." ] ; then
  echo "${progname}: you must specify a subdirectory" >&2
  exit 1
fi

if [ ! -w . ] ; then
  echo "${progname}: cannot write archive file to current directory." >&2
  exit 1
fi

dirsize="$(du -s $1 | awk '{print $1}')"

if [ $dirsize -gt $maxarchivedir ] ; then
  echo -n "Warning: directory $1 is $dirsize blocks. Proceed? [n] " 
  read answer
  answer="$(echo $answer | tr '[:upper:]' '[:lower:]' | cut -c1)"
  if [ "$answer" != "y" ] ; then
    echo "${progname}: archive of directory $1 canceled." >&2
    exit 0
  fi
fi

archivename="$1.tgz"

if tar cf - $1 | $compress > $archivename ; then
  echo "Directory $1 archived as $archivename"
else
  echo "Warning: tar encountered errors archiving $1"
fi

exit 0
```

## #53 Downloading Files via FTP
**Explicacion del codigo:** Un script que dada una URL de estilo ftp, la abre e intenta obtener el archivo usando ftp anónimo. 

>Codigo a corregir:
```sh
#!/bin/bash

anonpass="$LOGNAME@$(hostname)"
if [ $# -ne 1 ] ; then
 echo "Usage: $0 ftp://..." >&2
 exit 1
fi

if [ "$(echo $1 | cut -c1-6)" != "ftp://" ] ; then
 echo "$0: Malformed url. I need it to start with ftp://" >&2
 exit 1
fi
server="$(echo $1 | cut -d/ -f3)"
filename="$(echo $1 | cut -d/ -f4-)"
basefile="$(basename $filename)"
echo ${0}: Downloading $basefile from server $server
X ftp -np << EOF
open $server
user ftp $anonpass
get "$filename" "$basefile"
quit
EOF
if [ $? -eq 0 ] ; then
 ls -l $basefile
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

anonpass="$LOGNAME@$(hostname)"

if [ $# -ne 1 ] ; then
  echo "Usage: $0 ftp://..." >&2
  exit 1
fi

if [ "$(echo $1 | cut -c1-6)" != "ftp://" ] ; then
  echo "$0: Malformed url. I need it to start with ftp://" >&2; 
  exit 1
fi

server="$(echo $1 | cut -d/ -f3)"
filename="$(echo $1 | cut -d/ -f4-)"
basefile="$(basename $filename)"

echo ${0}: Downloading $basefile from server $server

ftp -np << EOF
open $server
user ftp $anonpass
get "$filename" "$basefile"
quit
EOF

if [ $? -eq 0 ] ; then
  ls -l $basefile
fi

exit 0
```

## #54 Extracting URLs from a Web Page
**Explicacion del codigo:** Un script que dada una URL, devuelve todo lo que es relevante y enlaces absolutos. Tiene tres opciones: -d para generar el primario dominios de cada enlace, -i para enumerar solo aquellos enlaces que son interno al sitio (es decir, otras páginas en el mismo sitio), y -x para producir solo enlaces externos.


>Codigo a corregir:
```sh
#!/bin/bash
# getlinks--Given a URL, returns all of its relative and absolute links.
# Has three options: -d to generate the primary domains of every link,
# -i to list just those links that are internal to the site (that is,
# other pages on the same site), and -x to produce external links only
# (the opposite of -i).
if [ $# -eq 0 ] ; then
 echo "Usage: $0 [-d|-i|-x] url" >&2
 echo "-d=domains only, -i=internal refs only, -x=external only" >&2
 exit 1
fi
if [ $# -gt 1 ] ; then
 case "$1" in
 -d) lastcmd="cut -d/ -f3|sort|uniq"
 shift
 ;;
 -r) basedomain="http://$(echo $2 | cut -d/ -f3)/"
 lastcmd="grep \"^$basedomain\"|sed \"s|$basedomain||g\"|sort|uniq"
 shift
 ;;
 -a) basedomain="http://$(echo $2 | cut -d/ -f3)/"
 lastcmd="grep -v \"^$basedomain\"|sort|uniq"
 shift
 ;;
 *) echo "$0: unknown option specified: $1" >&2
 exit 1
 esac
else
 lastcmd="sort|uniq"
fi
lynx -dump "$1"|\
 sed -n '/^References$/,$p'|\
 grep -E '[[:digit:]]+\.'|\
 awk '{print $2}'|\
 cut -d\? -f1|\
 eval $lastcmd
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

if [ $# -eq 0 ] ; then
  echo "Usage: $0 [-d|-i|-x] url"  >&2
  echo "-d=domains only, -i=internal refs only, -x=external only" >&2
  exit 1
fi

if [ $# -gt 1 ] ; then
  case "$1" in
    -d) lastcmd="cut -d/ -f3 | sort | uniq"
        shift
        ;;
    -r) basedomain="http://$(echo $2 | cut -d/ -f3)/"
        lastcmd="grep \"^$basedomain\" | sed \"s|$basedomain||g\" | sort |\
	     uniq"
        shift
        ;;
    -a) basedomain="http://$(echo $2 | cut -d/ -f3)/"
        lastcmd="grep -v \"^$basedomain\" | sort | uniq"
        shift
        ;;
     *) echo "$0: unknown option specified: $1" >&2; exit 1
  esac
else
  lastcmd="sort | uniq"
fi

lynx -dump "$1" | \
sed -n '/^References$/,$p' | \
  grep -E '[[:digit:]]+\.' | \
  awk '{print $2}' | \
  cut -d\? -f1 | \
eval $lastcmd

exit 0
```

## #56 ZIP Code Lookup
**Explicacion del codigo:** Un script que mediante el codigo postal que el usuario ingrese este logre reconocer a que estado o ciudad pertenece este obteniendo la informacion de una pagina web.

>Codigo a corregir:
```sh
#!/bin/bash
# zipcode--Given a ZIP code, identifies the city and state. Use city-data.com,
# which has every ZIP code configured as its own web page.
baseURL="http://www.city-data.com/zips"
/bin/echo -n "ZIP code $1 is in "
curl -s -dump "$baseURL/$1.html" | \
 grep -i '<title>' | \
 cut -d\( -f2 | cut -d\) -f1
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

baseURL="http://www.city-data.com/zips"

/bin/echo -n "ZIP code $1 is in "

curl -s -dump "$baseURL/$1.html" | \
   grep -i '<title>' | \
   cut -d\( -f2 | cut -d\) -f1

exit 0
```

## #58 Keeping Track of the Weather
**Explicacion del codigo:** Un script similar al anterior ya que tambien pide el coigo postal al usuario solo que ahora no dara como resultado la region si no como se encuentra el clima en icha region. 

>Codigo a corregir:
```sh
#!/bin/bash
# weather--Uses the Wunderground API to get the weather for a given ZIP code
if [ $# -ne 1 ]; then
 echo "Usage: $0 <zipcode>"
 exit 1
fi
apikey="b03fdsaf3b2e7cd23" # Not a real API key--you need your own.
 weather=`curl -s \
 "https://api.wunderground.com/api/$apikey/conditions/q/$1.xml"`
 state=`xmllint --xpath \
 //response/current_observation/display_location/full/text\(\) \
 <(echo $weather)`
zip=`xmllint --xpath \
 //response/current_observation/display_location/zip/text\(\) \
 <(echo $weather)`
current=`xmllint --xpath \
 //response/current_observation/temp_f/text\(\) \
 <(echo $weather)`
186 Chapter 7
condition=`xmllint --xpath \
 //response/current_observation/weather/text\(\) \
 <(echo $weather)`
echo $state" ("$zip") : Current temp "$current"F and "$condition" outside."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

if [ $# -ne 1 ]; then
  echo "Usage: $0 <zipcode>"
  exit 1
fi

apikey="b0304b43b2e7cd23" 

weather=`curl -s \
    "https://api.wunderground.com/api/$apikey/conditions/q/$1.xml"`
state=`xmllint --xpath \
     //response/current_observation/display_location/full/text\(\) \
     <(echo $weather)`
zip=`xmllint --xpath \
     //response/current_observation/display_location/zip/text\(\) \
     <(echo $weather)`
current=`xmllint --xpath \
     //response/current_observation/temp_f/text\(\) \
     <(echo $weather)`
condition=`xmllint --xpath \
     //response/current_observation/weather/text\(\) \
     <(echo $weather)`

echo $state" ("$zip") : Current temp "$current"F and "$condition" outside." 
```

## #59 Digging Up Movie Info from IMDb
**Explicacion del codigo:** Un script que dado el titulo de una pelicula o un titulo de tv este devuelva una lista de coincidencias. Si el usuario especifica un número de índice numérico de IMDb, sin embargo si no lo hace, devolvera la sinopsis de la película en su lugar. Aqui se utiliza una base de datos sobre peliculas la cual es muy util para este script.

>Codigo a corregir:
```sh
#!/bin/bash
# moviedata--Given a movie or TV title, returns a list of matches. If the user
# specifies an IMDb numeric index number, however, returns the synopsis of
# the film instead. Uses the Internet Movie Database.
titleurl="http://www.imdb.com/title/tt"
imdburl="http://www.imdb.com/find?s=tt&exact=true&ref_=fn_tt_ex&q="
tempout="/tmp/moviedata.$$"
 summarize_film()
{
 # Produce an attractive synopsis of the film.
 grep "<title>" $tempout | sed 's/<[^>]*>//g;s/(more)//'
 grep --color=never -A2 '<h5>Plot:' $tempout | tail -1 | \
 cut -d\< -f1 | fmt | sed 's/^/ /'
 exit 0
}
trap "rm -f $tempout" 0 1 15
if [ $# -eq 0 ] ; then
 echo "Usage: $0 {movie title | movie ID}" >&2
 exit 1
fi
#########
# Checks whether we're asking for a title by IMDb title number
nodigits="$(echo $1 | sed 's/[[:digit:]]*//g')"
if [ $# -eq 1 -a -z "$nodigits" ] ; then
 lynx -source "$titleurl$1/combined" > $tempout
 summarize_film
 exit 0
fi
188 Chapter 7
##########
# It's not an IMDb title number, so let's go with the search...
fixedname="$(echo $@ | tr ' ' '+')" # for the URL
url="$imdburl$fixedname"
 lynx -source $imdburl$fixedname > $tempout
# No results?
 fail="$(grep --color=never '<h1 class="findHeader">No ' $tempout)"
# If there's more than one matching title...
if [ ! -z "$fail" ] ; then
 echo "Failed: no results found for $1"
 exit 1
elif [ ! -z "$(grep '<h1 class="findHeader">Displaying' $tempout)" ] ; then
 grep --color=never '/title/tt' $tempout | \
 sed 's/</\
</g' | \
 grep -vE '(.png|.jpg|>[ ]*$)' | \
 grep -A 1 "a href=" | \
 grep -v '^--$' | \
 sed 's/<a href="\/title\/tt//g;s/<\/a> //' | \
 awk '(NR % 2 == 1) { title=$0 } (NR % 2 == 0) { print title " " $0 }' | \
 sed 's/\/.*>/: /' | \
 sort
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

titleurl="http://www.imdb.com/title/tt"
imdburl="http://www.imdb.com/find?s=tt&exact=true&ref_=fn_tt_ex&q="
tempout="/tmp/moviedata.$$"

summarize_film()
{

   grep "<title>" $tempout | sed 's/<[^>]*>//g;s/(more)//'

   grep --color=never -A2 '<h5>Plot:' $tempout | tail -1 | \
     cut -d\< -f1 | fmt | sed 's/^/    /'

   exit 0
}

trap "rm -f $tempout" 0 1 15

if [ $# -eq 0 ] ; then
  echo "Usage: $0 {movie title | movie ID}" >&2
  exit 1
fi

nodigits="$(echo $1 | sed 's/[[:digit:]]*//g')"

if [ $# -eq 1 -a -z "$nodigits" ] ; then
  lynx -source "$titleurl$1/combined" > $tempout
  summarize_film
  exit 0
fi

fixedname="$(echo $@ | tr ' ' '+')"	

url="$imdburl$fixedname"

lynx -source $imdburl$fixedname > $tempout

fail="$(grep --color=never '<h1 class="findHeader">No ' $tempout)"


if [ ! -z "$fail" ] ; then
  echo "Failed: no results found for $1"
  exit 1
elif [ ! -z "$(grep '<h1 class="findHeader">Displaying' $tempout)" ] ; then
  grep --color=never '/title/tt' $tempout | \
  sed 's/</\
</g' | \
  grep -vE '(.png|.jpg|>[ ]*$)' | \
  grep -A 1 "a href=" | \
  grep -v '^--$' | \
  sed 's/<a href="\/title\/tt//g;s/<\/a> //' | \
   awk '(NR % 2 == 1) { title=$0 } (NR % 2 == 0) { print title " " $0 }' | \
  sed 's/\/.*>/: /' | \
  sort
fi


exit 0
```

## #60 Calculating Currency Values
**Explicacion del codigo:** Un script que convierte una cantidad dada por el usuario a una moneda tambien dada por el usuario a traves del convertidor de divisas del buscador google.

>Codigo a corregir:
```sh
#!/bin/bash
# convertcurrency--Given an amount and base currency, converts it
# to the specified target currency using ISO currency identifiers.
# Uses Google's currency converter for the heavy lifting:
# http://www.google.com/finance/converter
if [ $# -eq 0 ]; then
 echo "Usage: $(basename $0) amount currency to currency"
 echo "Most common currencies are CAD, CNY, EUR, USD, INR, JPY, and MXN"
 echo "Use \"$(basename $0) list\" for a list of supported currencies."
fi
if [ $(uname) = "Darwin" ]; then
 LANG=C # For an issue on OS X with invalid byte sequences and lynx
fi
 url="https://www.google.com/finance/converter"
tempfile="/tmp/converter.$$"
 lynx=$(which lynx)
# Since this has multiple uses, let's grab this data before anything else.
currencies=$($lynx -source "$url" | grep "option value=" | \
 cut -d\" -f2- | sed 's/">/ /' | cut -d\( -f1 | sort | uniq)
Web and Internet Users 191
########### Deal with all non-conversion requests.
if [ $# -ne 4 ] ; then
 if [ "$1" = "list" ] ; then
 # Produce a listing of all currency symbols known by the converter.
 echo "List of supported currencies:"
 echo "$currencies"
 fi
 exit 0
fi
########### Now let's do a conversion.
if [ $3 != "to" ] ; then
 echo "Usage: $(basename $0) value currency TO currency"
 echo "(use \"$(basename $0) list\" to get a list of all currency values)"
 exit 0
fi
amount=$1
basecurrency="$(echo $2 | tr '[:lower:]' '[:upper:]')"
targetcurrency="$(echo $4 | tr '[:lower:]' '[:upper:]')"
# And let's do it--finally!
$lynx -source "$url?a=$amount&from=$basecurrency&to=$targetcurrency" | \
 grep 'id=currency_converter_result' | sed 's/<[^>]*>//g'
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

if [ $# -eq 0 ]; then
   echo "Usage: $(basename $0) amount currency to currency"
   echo "Most common currencies are CAD, CNY, EUR, USD, INR, JPY, and MXN"
   echo "Use \"$(basename $0) list\" for the full list of supported currencies"
fi

if [ $(uname) = "Darwin" ]; then
  LANG=C 
fi
     url="https://www.google.com/finance/converter"
tempfile="/tmp/converter.$$"
    lynx=$(which lynx)


currencies=$($lynx -source "$url" | grep "option  value=" | \
    cut -d\" -f2- | sed 's/">/ /' | cut -d\( -f1 | sort | uniq)

if [ $# -ne 4 ] ; then
  if [ "$1" = "list" ] ; then
    echo "List of supported currencies:"
    echo "$currencies"
  fi
  exit 0
fi

if [ $3 != "to" ] ; then
  echo "Usage: $(basename $0) value currency TO currency"
  echo "(use \"$(basename $0) list\" to get a list of all currency values)"
  exit 0
fi

amount=$1
basecurrency="$(echo $2 | tr '[:lower:]' '[:upper:]')"
targetcurrency="$(echo $4 | tr '[:lower:]' '[:upper:]')"

$lynx -source "$url?a=$amount&from=$basecurrency&to=$targetcurrency" | \
  grep 'id=currency_converter_result' | sed 's/<[^>]*>//g'

exit 0
```

## #61 Retrieving Bitcoin Address Information
**Explicacion del codigo:** Un script que recopila los datos e una direccion dada de bitcoin y nos muestra la informacion mas util.

>Codigo a corregir:
```sh
#!/bin/bash
# getbtcaddr--Given a Bitcoin address, reports useful information
Web and Internet Users 193
if [ $# -ne 1 ]; then
 echo "Usage: $0 <address>"
 exit 1
fi
base_url="https://blockchain.info/q/"
balance=$(curl -s $base_url"addressbalance/"$1)
recv=$(curl -s $base_url"getreceivedbyaddress/"$1)
sent=$(curl -s $base_url"getsentbyaddress/"$1)
first_made=$(curl -s $base_url"addressfirstseen/"$1)
echo "Details for address $1"
echo -e "\tFirst seen: "$(date -d @$first_made)
echo -e "\tCurrent balance: "$balance
echo -e "\tSatoshis sent: "$sent
echo -e "\tSatoshis recv: "$recv
```
>Codigo corregido:
```sh
#!/bin/bash

if [ $# -ne 1 ]; then
  echo "Usage: $0 <address>"
  exit 1
fi

base_url="https://blockchain.info/q/"

balance=$(curl -s $base_url"addressbalance/"$1)
recv=$(curl -s $base_url"getreceivedbyaddress/"$1)
sent=$(curl -s $base_url"getsentbyaddress/"$1)
first_made=$(curl -s $base_url"addressfirstseen/"$1)

echo "Details for address $1"
echo -e "\tFirst seen: "$(date -d @$first_made)
echo -e "\tCurrent balance: "$balance
echo -e "\tSatoshis sent: "$sent
echo -e "\tSatoshis recv: "$recv
```

## #63 Seeing the CGI Environment
**Explicacion del codigo:** Un script que muestra el entorno de tiempo de ejecución CGI, tal como se le da a cualquier script CGI en este sistema.

>Codigo a corregir:
```sh
#!/bin/bash
# showCGIenv--Displays the CGI runtime environment, as given to any
# CGI script on this system
echo "Content-type: text/html"
echo ""
# Now the real information...
echo "<html><body bgcolor=\"white\"><h2>CGI Runtime Environment</h2>"
echo "<pre>"
 env || printenv
echo "</pre>"
echo "<h3>Input stream is:</h3>"
echo "<pre>"
cat -
echo "(end of input stream)</pre></body></html>"
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

echo "Content-type: text/html"
echo ""

echo "<html><body bgcolor=\"white\"><h2>CGI Runtime Environment</h2>"
echo "<pre>"
env || printenv
echo "</pre>"
echo "<h3>Input stream is:</h3>"
echo "<pre>"
cat -
echo "(end of input stream)</pre></body></html>"

exit 0
```

## #64 Logging Web Events
**Explicacion del codigo:** Un script que mediante una solicitud de busqueda dada registra el patrón y luego envíe la secuencia completa al sistema de búsqueda DuckDuckGo real.

>Codigo a corregir:
```sh
#!/bin/bash
# log-duckduckgo-search--Given a search request, logs the pattern and then
# feeds the entire sequence to the real DuckDuckGo search system
# Make sure the directory path and file listed as logfile are writable by
# the user that the web server is running as.
logfile="/var/www/wicked/scripts/searchlog.txt"
if [ ! -f $logfile ] ; then
 touch $logfile
 chmod a+rw $logfile
fi
if [ -w $logfile ] ; then
 echo "$(date): $QUERY_STRING" | sed 's/q=//g;s/+/ /g' >> $logfile
fi
echo "Location: https://duckduckgo.com/html/?$QUERY_STRING"
echo ""
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

logfile="/var/www/wicked/scripts/searchlog.txt"

if [ ! -f $logfile ] ; then
  touch $logfile
  chmod a+rw $logfile
fi

if [ -w $logfile ] ; then
  echo "$(date): $QUERY_STRING" | sed 's/q=//g;s/+/ /g' >> $logfile
fi

echo "Location: https://duckduckgo.com/html/?$QUERY_STRING"
echo ""

exit 0
```

## #65 Building Web Pages on the Fly
**Explicacion del codigo:** Un script que crea una página web sobre la marcha para mostrar las últimas tiras de comic de la caricatura "Kevin and Kell" de Bill Holbrook.

>Codigo a corregir:
```sh
#!/bin/bash
# kevin-and-kell--Builds a web page on the fly to display the latest
# strip from the cartoon "Kevin and Kell" by Bill Holbrook.
# <Strip referenced with permission of the cartoonist>
month="$(date +%m)"
 day="$(date +%d)"
 year="$(date +%y)"
echo "Content-type: text/html"
echo ""
echo "<html><body bgcolor=white><center>"
echo "<table border=\"0\" cellpadding=\"2\" cellspacing=\"1\">"
echo "<tr bgcolor=\"#000099\">"
echo "<th><font color=white>Bill Holbrook's Kevin &amp; Kell</font></th></tr>"
echo "<tr><td><img "
# Typical URL: http://www.kevinandkell.com/2016/strips/kk20160804.jpg
/bin/echo -n " src=\"http://www.kevinandkell.com/20${year}/"
echo "strips/kk20${year}${month}${day}.jpg\">"
echo "</td></tr><tr><td align=\"center\">"
echo "&copy; Bill Holbrook. Please see "
echo "<a href=\"http://www.kevinandkell.com/\">kevinandkell.com</a>"
echo "for more strips, books, etc."
echo "</td></tr></table></center></body></html>"
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

month="$(date +%m)"
  day="$(date +%d)"
 year="$(date +%y)"

echo "Content-type: text/html"
echo ""

echo "<html><body bgcolor=white><center>"
echo "<table border=\"0\" cellpadding=\"2\" cellspacing=\"1\">"
echo "<tr bgcolor=\"#000099\">"
echo "<th><font color=white>Bill Holbrook's Kevin &amp; Kell</font></th></tr>"
echo "<tr><td><img "

/bin/echo -n " src=\"http://www.kevinandkell.com/20${year}/"
echo "strips/kk20${year}${month}${day}.jpg\">"
echo "</td></tr><tr><td align=\"center\">"
echo "&copy; Bill Holbrook. Please see "
echo "<a href=\"http://www.kevinandkell.com/\">kevinandkell.com</a>"
echo "for more strips, books, etc."
echo "</td></tr></table></center></body></html>"

exit 0
```

## #66 Turning Web Pages into Email Messages
**Explicacion del codigo:** Un script que pueda convertir paginas web en mensajes de correo electronico implementando un script hecho anteriormente.

>Codigo a corregir:
```sh
#!/bin/bash
# getdope--Grabs the latest column of "The Straight Dope."
# Set it up in cron to be run every day, if so inclined.
now="$(date +%y%m%d)"
start="http://www.straightdope.com/ "
to="testing@yourdomain.com" # Change this as appropriate.
# First, get the URL of the current column.
 URL="$(curl -s "$start" | \
grep -A1 'teaser' | sed -n '2p' | \
cut -d\" -f2 | cut -d\" -f1)"
# Now, armed with that data, produce the email.
( cat << EOF
Subject: The Straight Dope for $(date "+%A, %d %B, %Y")
From: Cecil Adams <dont@reply.com>
Content-type: text/html
To: $to
EOF
curl "$URL"
) | /usr/sbin/sendmail -t
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

now="$(date +%y%m%d)"
start="http://www.straightdope.com/ "
to="testing@yourdomain.com" 

URL="$(curl -s "$start" | \
  grep -A1 'teaser' | sed -n '2p' | \
  cut -d\" -f2 | cut -d\" -f1)"

( cat << EOF
Subject: The Straight Dope for $(date "+%A, %d %B, %Y")
From: Cecil Adams <dont@reply.com>
Content-type: text/html
To: $to
EOF

curl "$URL"
) | /usr/sbin/sendmail -t

exit 0
```

## #68 Displaying Random Text
**Explicacion del codigo:** Un scrip que dado un archivo de datos de una línea por entrada, este elija aleatoriamente una línea y la muestre.

>Codigo a corregir:
```sh
#!/bin/bash
# randomquote--Given a one-line-per-entry datafile,
# randomly picks one line and displays it. Best used
# as an SSI call within a web page.
awkscript="/tmp/randomquote.awk.$$"
if [ $# -ne 1 ] ; then
 echo "Usage: randomquote datafilename" >&2
 exit 1
elif [ ! -r "$1" ] ; then
 echo "Error: quote file $1 is missing or not readable" >&2
 exit 1
fi
trap "$(which rm) -f $awkscript" 0
cat << "EOF" > $awkscript
BEGIN { srand() }
 { s[NR] = $0 }
END { print s[randint(NR)] }
function randint(n) { return int (n * rand() ) + 1 }
EOF
awk -f $awkscript < "$1"
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

awkscript="/tmp/randomquote.awk.$$"

if [ $# -ne 1 ] ; then
  echo "Usage: randomquote datafilename" >&2
  exit 1
elif [ ! -r "$1" ] ; then
  echo "Error: quote file $1 is missing or not readable" >&2
  exit 1
fi

trap "$(which rm) -f $awkscript" 0

cat << "EOF" > $awkscript
BEGIN { srand() }
      { s[NR] = $0 } 
END   { print s[randint(NR)] } 
function randint(n) { return int (n * rand() ) + 1 }
EOF

awk -f $awkscript < "$1"

exit 0
```

## #69 Identifying Broken Internal Links
**Explicacion del codigo:** Un scrip que verifica todas las URL internas en un sitio web, informando cualquier error en el archivo "traverse.errors".

>Codigo a corregir:
```sh
#!/bin/bash
# checklinks--Traverses all internal URLs on a website, reporting
# any errors in the "traverse.errors" file
# Remove all the lynx traversal output files upon completion.
trap "$(which rm) -f traverse.dat traverse2.dat" 0
if [ -z "$1" ] ; then
 echo "Usage: checklinks URL" >&2
 exit 1
fi
baseurl="$(echo $1 | cut -d/ -f3 | sed 's/http:\/\///')"
lynx -traversal -accept_all_cookies -realm "$1" > /dev/null
if [ -s "traverse.errors" ] ; then
 /bin/echo -n $(wc -l < traverse.errors) errors encountered.
 echo Checked $(grep '^http' traverse.dat | wc -l) pages at ${1}:
 sed "s|$1||g" < traverse.errors
 mv traverse.errors ${baseurl}.errors
 echo "A copy of this output has been saved in ${baseurl}.errors"
else
 /bin/echo -n "No errors encountered. ";
 echo Checked $(grep '^http' traverse.dat | wc -l) pages at ${1}
fi
if [ -s "reject.dat" ]; then
 mv reject.dat ${baseurl}.rejects
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

trap "`which rm` -f traverse.dat traverse2.dat" 0

if [ -z "$1" ] ; then
  echo "Usage: checklinks URL" >&2 ; exit 1
fi

baseurl="$(echo $1 | cut -d/ -f3 | sed 's/http:\/\///')"

lynx -traversal -accept_all_cookies -realm "$1" > /dev/null

if [ -s "traverse.errors" ] ; then
 echo -n $(wc -l < traverse.errors) errors encountered.
 echo  Checked $(grep '^http' traverse.dat | wc -l) pages at ${1}:
 sed "s|$1||g" < traverse.errors
 mv traverse.errors ${baseurl}.errors
 echo "(A copy of this output has been saved in ${baseurl}.errors)"
else
 echo -n "No errors encountered. ";
 echo Checked $(grep '^http' traverse.dat | wc -l) pages at ${1}  
fi

if [ -s "reject.dat" ]; then
 mv reject.dat ${baseurl}.rejects
fi

exit 0
```

## #81 Producing Summary Listings of iTunes Libraries
**Explicacion del codigo:** Un script que enumera tu biblioteca de iTunes de forma eficiente y atractiva adecuada para compartir con otros, o para sincronizarse.

>Codigo a corregir:
```sh
#!/bin/bash
# ituneslist--Lists your iTunes library in a succinct and attractive
# manner, suitable for sharing with others, or for synchronizing
# (with diff) iTunes libraries on different computers and laptops
itunehome="$HOME/Music/iTunes"
ituneconfig="$itunehome/iTunes Music Library.xml"
 musiclib="/$(grep '>Music Folder<' "$ituneconfig" | cut -d/ -f5- | \
 cut -d\< -f1 | sed 's/%20/ /g')"
echo "Your library is at $musiclib"
if [ ! -d "$musiclib" ] ; then
 echo "$0: Confused: Music library $musiclib isn't a directory?" >&2
 exit 1
fi
exec find "$musiclib" -type d -mindepth 2 -maxdepth 2 \! -name '.*' -print \
 | sed "s|$musiclib/||"
```
>Codigo corregido:
```sh
#!/bin/bash

itunehome="$HOME/Music/iTunes"
ituneconfig="$itunehome/iTunes Music Library.xml"

musiclib="/$(grep '>Music Folder<' "$ituneconfig" | cut -d/ -f5- | \
   cut -d\< -f1 | sed 's/%20/ /g')"

echo "Your library is at $musiclib"

if [ ! -d "$musiclib" ] ; then
  echo "$0: Confused: Music library $musiclib isn't a directory?" >&2
  exit 1
fi

exec find "$musiclib" -type d -mindepth 2 -maxdepth 2 \! -name '.*' -print |
   sed "s|$musiclib/||"
```

## #82 Fixing the open Command
**Explicacion del codigo:** Un script que arregla el comando open de Mac OS X para hacerlo aún más útil. Por defecto, 'open' inicia la aplicación apropiada para un archivo o directorio específico basado en los enlaces Aqua, y tiene una capacidad limitada para iniciar aplicaciones si están en el directorio /Applications dir.

>Codigo a corregir:
```sh
#!/bin/bash
# open2--A smart wrapper for the cool OS X 'open' command
# to make it even more useful. By default, 'open' launches the
# appropriate application for a specified file or directory
# based on the Aqua bindings, and it has a limited ability to
# launch applications if they're in the /Applications dir.
# First, whatever argument we're given, try it directly.
 if ! open "$@" >/dev/null 2>&1 ; then
 if ! open -a "$@" >/dev/null 2>&1 ; then
 # More than one arg? Don't know how to deal with it--quit.
 if [ $# -gt 1 ] ; then
 echo "open: More than one program not supported" >&2
 exit 1
 else
 case $(echo $1 | tr '[:upper:]' '[:lower:]') in
 activ*|cpu ) app="Activity Monitor" ;;
 addr* ) app="Address Book" ;;
 chat ) app="Messages" ;;
 dvd ) app="DVD Player" ;;
 excel ) app="Microsoft Excel" ;;
 info* ) app="System Information" ;;
 prefs ) app="System Preferences" ;;
 qt|quicktime ) app="QuickTime Player" ;;
 word ) app="Microsoft Word" ;;
 * ) echo "open: Don't know what to do with $1" >&2
 exit 1
 esac
 echo "You asked for $1 but I think you mean $app." >&2
 open -a "$app"
 fi
 fi
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

if ! open "$@" >/dev/null 2>&1 ; then
  if ! open -a "$@" >/dev/null 2>&1 ; then

    if [ $# -gt 1 ] ; then
      echo "open: Can't figure out how to open or launch $@More than one program not supported" >&2
      exit 1
    else
        case $(echo $1 | tr '[:upper:]' '[:lower:]') in
        activ*|cpu   ) app="Activity Monitor"           ;;
        addr*        ) app="Address Book"               ;;
        chat         ) app="Messages"                   ;;
        dvd          ) app="DVD Player"                 ;;
        excel        ) app="Microsoft Excel"            ;;
        info*        ) app="System Information"         ;;
        prefs        ) app="System Preferences"         ;;
        qt|quicktime ) app="QuickTime Player"           ;;
        word         ) app="Microsoft Word"             ;;        
        * ) echo "open: Don't know what to do with $1" >&2
            exit 1
      esac
      echo "You asked for $1 but I think you mean $app." >&2
      open -a "$app"
    fi
  fi
fi

exit 0
```

## #83 Unscramble: A Word Game
**Explicacion del codigo:** Un script que recrea el clasico juego de scrable mediante una variable en donde se guarda una palabra elegida al azar y despues pide al usuario que adivine cual era la palabra o frase original.

>Codigo a corregir:
```sh
#!/bin/bash
# unscramble--Picks a word, scrambles it, and asks the user to guess
# what the original word (or phrase) was
wordlib="/usr/lib/games/long-words.txt"
scrambleword()
{
 # Pick a word randomly from the wordlib and scramble it.
 # Original word is $match, and scrambled word is $scrambled.
 match="$(randomquote $wordlib)"
 echo "Picked out a word!"
 len=${#match}
 scrambled=""; lastval=1
 for (( val=1; $val < $len ; ))
 do
 if [ $(($RANDOM % 2)) -eq 1 ] ; then
 scrambled=$scrambled$(echo $match | cut -c$val)
 else
 scrambled=$(echo $match | cut -c$val)$scrambled
 fi
 val=$(( $val + 1 ))
 done
}
if [ ! -r $wordlib ] ; then
 echo "$0: Missing word library $wordlib" >&2
 echo "(online: http://www.intuitive.com/wicked/examples/long-words.txt" >&2
 echo "save the file as $wordlib and you're ready to play!)" >&2
 exit 1
fi
newgame=""; guesses=0; correct=0; total=0
 until [ "$guess" = "quit" ] ; do
 scrambleword
276 Chapter 12
 echo ""
 echo "You need to unscramble: $scrambled"
 guess="??" ; guesses=0
 total=$(( $total + 1 ))
 while [ "$guess" != "$match" -a "$guess" != "quit" -a "$guess" != "next" ]
 do
 echo ""
 /bin/echo -n "Your guess (quit|next) : "
 read guess
 if [ "$guess" = "$match" ] ; then
 guesses=$(( $guesses + 1 ))
 echo ""
 echo "*** You got it with tries = ${guesses}! Well done!! ***"
 echo ""
 correct=$(( $correct + 1 ))
 elif [ "$guess" = "next" -o "$guess" = "quit" ] ; then
 echo "The unscrambled word was \"$match\". Your tries: $guesses"
 else
 echo "Nope. That's not the unscrambled word. Try again."
 guesses=$(( $guesses + 1 ))
 fi
 done
done
echo "Done. You correctly figured out $correct out of $total scrambled words."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

wordlib="/usr/lib/games/long-words.txt"

scrambleword()
{

  match="$(randomquote $wordlib)"

  echo "Picked out a word!"

  len=$(echo $match | wc -c | sed 's/[^[:digit:]]//g')
  scrambled=""; lastval=1

  for (( val=1; $val < $len ; )) 
  do
    if [ $(($RANDOM % 2)) -eq 1 ] ; then
      scrambled=$scrambled$(echo $match | cut -c$val)
    else
      scrambled=$(echo $match | cut -c$val)$scrambled
    fi
    val=$(( $val + 1 ))
  done
}

if [ ! -r $wordlib ] ; then
  echo "$0: Missing word library $wordlib" >&2
  echo "(online: http://www.intuitive.com/wicked/examples/long-words.txt" >&2
  echo "save the file as $wordlib and you're ready to play!)" >&2
  exit 1
fi

newgame=""; guesses=0; correct=0; total=0 

until [ "$guess" = "quit" ] ; do

  scrambleword

  echo ""
  echo "You need to unscramble: $scrambled"

  guess="??" ; guesses=0
  total=$(( $total + 1 ))

  while [ "$guess" != "$match" -a "$guess" != "quit" -a "$guess" != "next" ] 
  do
    echo ""
    /bin/echo -n "Your guess (quit|next) : "
    read guess
 
    if [ "$guess" = "$match" ] ; then
      guesses=$(( $guesses + 1 ))
      echo ""
      echo "*** You got it with tries = ${guesses}!  Well done!! ***"  
      echo ""
      correct=$(( $correct + 1 ))
    elif [ "$guess" = "next" -o "$guess" = "quit" ] ; then
      echo "The unscrambled word was \"$match\". Your tries: $guesses"
    else
      echo "Nope. That's not the unscrambled word. Try again."
      guesses=$(( $guesses + 1 ))
    fi
  done
done

echo "Done. You correctly figured out $correct out of $total scrambled words."

exit 0
```

## #85 A State Capitals Quiz
**Explicacion del codigo:** Un script que crea un juego en el cual el script lee un archivo el cual se tiene que descargar para poder leer los 50 estados de los Estados Unidos para despues mostrar al usuario un estado al azar y despues el usuario debera intentar adivinar su capital.

>Codigo a corregir:
```sh
#!/bin/bash
# states--A state capital guessing game. Requires the state capitals
# data file state.capitals.txt.
db="/usr/lib/games/state.capitals.txt" # Format is State[tab]City.
if [ ! -r "$db" ] ; then
 echo "$0: Can't open $db for reading." >&2
 echo "(get state.capitals.txt" >&2
 echo "save the file as $db and you're ready to play!)" >&2
 exit 1
fi
guesses=0; correct=0; total=0
while [ "$guess" != "quit" ] ; do

 thiskey="$(randomquote $db)"

 # $thiskey is the selected line. Now let's grab state and city info, and
 # then also have "match" as the all-lowercase version of the city name.
 state="$(echo $thiskey | cut -d\ -f1 | sed 's/-/ /g')"
 city="$(echo $thiskey | cut -d\ -f2 | sed 's/-/ /g')"
 match="$(echo $city | tr '[:upper:]' '[:lower:]')"
 guess="??" ; total=$(( $total + 1 )) ;
 echo ""
 echo "What city is the capital of $state?"
 # Main loop where all the action takes place. Script loops until
 # city is correctly guessed or the user types "next" to
 # skip this one or "quit" to quit the game.
 while [ "$guess" != "$match" -a "$guess" != "next" -a "$guess" != "quit" ]
 do
 /bin/echo -n "Answer: "
 read guess
Shell Script Fun and Games 283
 if [ "$guess" = "$match" -o "$guess" = "$city" ] ; then
 echo ""
 echo "*** Absolutely correct! Well done! ***"
 correct=$(( $correct + 1 ))
 guess=$match
 elif [ "$guess" = "next" -o "$guess" = "quit" ] ; then
 echo ""
 echo "$city is the capital of $state." # What you SHOULD have known :)
 else
 echo "I'm afraid that's not correct."
 fi
 done
done
echo "You got $correct out of $total presented."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

db="/usr/lib/games/state.capitals.txt"    

if [ ! -r "$db" ] ; then
  echo "$0: Can't open $db for reading." >&2
  echo "(get http://www.intuitive.com/wicked/examples/state.capitals.txt" >&2
  echo "save the file as $db and you're ready to play!)" >&2
  exit 1
fi

guesses=0; correct=0; total=0

while [ "$guess" != "quit" ] ; do
  
  thiskey="$(randomquote $db)"

  state="$(echo $thiskey | cut -d\   -f1 | sed 's/-/ /g')"
   city="$(echo $thiskey | cut -d\   -f2 | sed 's/-/ /g')"
  match="$(echo $city | tr '[:upper:]' '[:lower:]')"

  guess="??" ; total=$(( $total + 1 )) ;

  echo ""
  echo "What city is the capital of $state?"

  while [ "$guess" != "$match" -a "$guess" != "next" -a "$guess" != "quit" ]
  do
    /bin/echo -n "Answer: "
    read guess

    if [ "$guess" = "$match" -o "$guess" = "$city" ] ; then
      echo ""
      echo "*** Absolutely correct!  Well done! ***"
      correct=$(( $correct + 1 ))
      guess=$match
    elif [ "$guess" = "next" -o "$guess" = "quit" ] ; then
      echo ""
      echo "$city is the capital of $state."  
    else
      echo "I'm afraid that's not correct."
    fi 
  done

done

echo "You got $correct out of $total presented."
exit 0
```

## #86 Is That Number a Prime?
**Explicacion del codigo:** Un script que mediante una serie de calculos matematicos determine si un numero ingresado por el usuario es primo o no.

>Codigo a corregir:
```sh
#!/bin/bash
# isprime--Given a number, ascertain whether it's a prime. This uses what's
# known as trial division: simply check whether any number from 2 to (n/2)
# divides into the number without a remainder.
 counter=2
remainder=1
if [ $# -eq 0 ] ; then
 echo "Usage: isprime NUMBER" >&2
 exit 1
fi
number=$1
# 3 and 2 are primes, 1 is not.
if [ $number -lt 2 ] ; then
 echo "No, $number is not a prime"
 exit 0
fi
# Now let's run some calculations.
 while [ $counter -le $(expr $number / 2) -a $remainder -ne 0 ]
do
 remainder=$(expr $number % $counter) # '/' is divide, '%' is remainder
 # echo " for counter $counter, remainder = $remainder"
 counter=$(expr $counter + 1)
done
if [ $remainder -eq 0 ] ; then
 echo "No, $number is not a prime"
else
 echo "Yes, $number is a prime"
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

  counter=2
remainder=1

if [ $# -eq 0 ] ; then
  echo "Usage: isprime NUMBER" >&2
  exit 1
fi

number=$1

if [ $number -lt 2 ] ; then
  echo "No, $number is not a prime" ; exit 0
fi

while [ $counter -le $(expr $number / 2) -a $remainder -ne 0 ]
do
  remainder=$(expr $number % $counter) 
  counter=$(expr $counter + 1)
done

if [ $remainder -eq 0 ] ; then
  echo "No, $number is not a prime"
else
  echo "Yes, $number is a prime"
fi
exit 0
```

## #89 Keeping Dropbox Running
**Explicacion del codigo:** Un script que simplemente verifica si la aplicacion de dropbox se esta ejecutando o no.

>Codigo a corregir:
```sh
#!/bin/bash
# startdropbox--Makes sure Dropbox is running on OS X
app="Dropbox.app"
verbose=1
running="$(ps aux | grep -i $app | grep -v grep)"
if [ "$1" = "-s" ] ; then # -s is for silent mode.
 verbose=0
fi
if [ ! -z "$running" ] ; then
 if [ $verbose -eq 1 ] ; then
 echo "$app is running with PID $(echo $running | cut -d\ -f2)"
 fi
else
 if [ $verbose -eq 1 ] ; then
 echo "Launching $app"
 fi
 open -a $app
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

app="Dropbox.app"
verbose=1

running="$(ps aux | grep -i $app | grep -v grep)"

if [ "$1" = "-s" ] ; then	
  verbose=0
fi

if [ ! -z "$running" ] ; then
  if [ $verbose -eq 1 ] ; then
    echo "$app is running with PID $(echo $running | cut -d\  -f2)"
  fi
else
  if [ $verbose -eq 1 ] ; then
    echo "Launching $app"
  fi
  open -a $app
fi

exit 0
```

## #90 Syncing Dropbox
**Explicacion del codigo:** Un script que sincroniza un conjunto de archivos o una carpeta específica con Dropbox. Se logra copiando la carpeta en ~/Dropbox o el conjunto de archivos en la carpeta de sincronización en Dropbox, luego inicie Dropbox.app como necesario.

>Codigo a corregir:
```sh
#!/bin/bash
# syncdropbox--Synchronize a set of files or a specified folder with Dropbox.
# This is accomplished by copying the folder into ~/Dropbox or the set of
# files into the sync folder in Dropbox and then launching Dropbox.app
# as needed.
302 Chapter 13
name="syncdropbox"
dropbox="$HOME/Dropbox"
sourcedir=""
targetdir="sync" # Target folder on Dropbox for individual files
# Check starting arguments.
if [ $# -eq 0 ] ; then
 echo "Usage: $0 [-d source-folder] {file, file, file}" >&2
 exit 1
fi
if [ "$1" = "-d" ] ; then
 sourcedir="$2"
 shift; shift
fi
# Validity checks
if [ ! -z "$sourcedir" -a $# -ne 0 ] ; then
 echo "$name: You can't specify both a directory and specific files." >&2
 exit 1
fi
if [ ! -z "$sourcedir" ] ; then
 if [ ! -d "$sourcedir" ] ; then
 echo "$name: Please specify a source directory with -d." >&2
 exit 1
 fi
fi
#######################
#### MAIN BLOCK
#######################
if [ ! -z "$sourcedir" ] ; then
 if [ -f "$dropbox/$sourcedir" -o -d "$dropbox/$sourcedir" ] ; then
 echo "$name: Specified source directory $sourcedir already exists." >&2
 exit 1
 fi
 echo "Copying contents of $sourcedir to $dropbox..."
 # -a does a recursive copy, preserving owner info, etc.
 cp -a "$sourcedir" $dropbox
else
 # No source directory, so we've been given individual files.
 if [ ! -d "$dropbox/$targetdir" ] ; then
 mkdir "$dropbox/$targetdir"
 if [ $? -ne 0 ] ; then
 echo "$name: Error encountered during mkdir $dropbox/$targetdir." >&2
 exit 1
 fi
 fi

Working with the Cloud 303
 # Ready! Let's copy the specified files.
 cp -p -v "$@" "$dropbox/$targetdir"
fi
# Now let's launch the Dropbox app to let it do the actual sync, if needed.
exec startdropbox -s

```
>Codigo corregido:
```sh
#!/bin/bash

name="syncdropbox"
dropbox="$HOME/Dropbox"
sourcedir=""
targetdir="sync"		

if [ $# -eq 0 ] ; then
  echo "Usage: $0 [-d source-folder] {file, file, file}" >&2 ; exit 1
fi 

if [ "$1" = "-d" ] ; then
  sourcedir="$2"
  shift; shift
fi

if [ ! -z "$sourcedir" -a $# -ne 0 ] ; then
  echo "$name: you can't specify both a directory and specific files." >&2 ; exit 1
fi

if [ ! -z "$sourcedir" ] ; then
  if [ ! -d "$sourcedir" ] ; then
    echo "$name: please specify a source directory with -d" >&2 ; exit 1
  fi
fi

if [ ! -z "$sourcedir" ] ; then
 if [ -f "$dropbox/$sourcedir" -o -d "$dropbox/$sourcedir" ] ; then
    echo "$name: specified source directory $sourcedir already exists in $dropbox" >&2 ; exit 1
  fi

  echo "Copying contents of $sourcedir to $dropbox..."
  cp -a "$sourcedir" $dropbox		

else

  if [ ! -d "$dropbox/$targetdir" ] ; then
    mkdir "$dropbox/$targetdir"
    if [ $? -ne 0 ] ; then
      echo "$name: Error encountered during mkdir $dropbox/$targetdir" >&2 ; exit 1
    fi
  fi
    
 cp -p -v $@ "$dropbox/$targetdir"
fi

exec startdropbox -s
```

## #91 Creating Slide Shows from Cloud Photo Streams
**Explicacion del codigo:** Un script que muestra al usuario una presentacion de diapositivas de fotos del directorio especificado utilizando la utilidad "display"

>Codigo a corregir:
```sh
#!/bin/bash
# slideshow--Displays a slide show of photos from the specified directory.
# Uses ImageMagick's "display" utility.
delay=2 # Default delay in seconds
 psize="1200x900>" # Preferred image size for display
if [ $# -eq 0 ] ; then
 echo "Usage: $(basename $0) watch-directory" >&2
 exit 1
fi
watch="$1"
if [ ! -d "$watch" ] ; then
 echo "$(basename $0): Specified directory $watch isn't a directory." >&2
 exit 1
fi
cd "$watch"
if [ $? -ne 0 ] ; then
 echo "$(basename $0): Failed trying to cd into $watch" >&2
 exit 1
fi
suffixes="$(file * | grep image | cut -d: -f1 | rev | cut -d. -f1 | \
 rev | sort | uniq | sed 's/^/\*./')"
if [ -z "$suffixes" ] ; then
 echo "$(basename $0): No images to display in folder $watch" >&2
 exit 1
fi
/bin/echo -n "Displaying $(ls $suffixes | wc -l) images from $watch "
 set -f ; echo "with suffixes $suffixes" ; set +f
display -loop 0 -delay $delay -resize $psize -backdrop $suffixes
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

delay=2			
psize="1200x900>"	

if [ $# -eq 0 ] ; then
  echo "Usage: $(basename $0) watch-directory" >&2 ; exit 1
fi

watch="$1"

if [ ! -d "$watch" ] ; then
  echo "$(basename $0): specified watch directory $watch isn't a directory" >&2 ; exit 1
fi

cd "$watch"

if [ $? -ne 0 ] ; then
  echo "$(basename $0): failed trying to cd into $watch" >&2 ; exit 1
fi

suffixes="$(file * | grep image | cut -d: -f1 | rev | cut -d. -f1 | \
   rev | sort | uniq | sed 's/^/\*./')"

if [ -z "$suffixes" ] ; then
  echo "$(basename $0): no images to display in folder $watch" >&2 ; exit 1
fi

/bin/echo -n "Displaying $(ls $suffixes | wc -l) images from $watch "
set -f ; echo with suffixes $suffixes ; set +f

display -loop 0 -delay $delay -resize $psize -backdrop $suffixes

exit 0
```
## #93 The Computer Says . . .
**Explicacion del codigo:** Un script que hace el comando "say" mas facil de trabajar el cual se utiliza para leer lo que el usuario le especifique en el script.
>Codigo a corregir:
```sh
#!/bin/bash
# sayit--Uses the "say" command to read whatever's specified (OS X only)
dosay="$(which say) --quality=127"
format="$(which fmt) -w 70"
voice="" # Default system voice
rate="" # Default to the standard speaking rate
demovoices()
{
 # Offer up a sample of each available voice.
 voicelist=$( say -v \? | grep "en_" | cut -c1-12 \
 | sed 's/ /_/;s/ //g;s/_$//')
 if [ "$1" = "list" ] ; then
 echo "Available voices: $(echo $voicelist | sed 's/ /, /g;s/_/ /g') \
 | $format"
 echo "HANDY TIP: use \"$(basename $0) demo\" to hear all the voices"
 exit 0
 fi
 for name in $voicelist ; do
 myname=$(echo $name | sed 's/_/ /')
 echo "Voice: $myname"
 $dosay -v "$myname" "Hello! I'm $myname. This is what I sound like."
 done
 exit 0
}
usage()
{
 echo "Usage: sayit [-v voice] [-r rate] [-f file] phrase"
 echo " or: sayit demo"
 exit 0
}
while getopts "df:r:v:" opt; do
 case $opt in
 d ) demovoices list ;;
 f ) input="$OPTARG" ;;
Working with the Cloud 311
 r ) rate="-r $OPTARG" ;;
 v ) voice="$OPTARG" ;;
 esac
done
shift $(($OPTIND - 1))
if [ $# -eq 0 -a -z "$input" ] ; then
 $dosay "Hey! You haven't given me any parameters to work with."
 echo "Error: no parameters specified. Specify a file or phrase."
 exit 0
fi
if [ "$1" = "demo" ] ; then
 demovoices
fi
if [ ! -z "$input" ] ; then
 $dosay $rate -v "$voice" -f $input
else
 $dosay $rate -v "$voice" "$*"
fi
exit 0

```
>Codigo corregido:
```sh
#!/bin/bash

dosay="`which say` --quality=127"
format="`which fmt` -w 70"

voice=""	    	     
rate=""		

demovoices()
{
  
   voicelist=$( say -v \? | grep "en_" | cut -c1-12 | 
       sed 's/ /_/;s/ //g;s/_$//')

   if [ "$1" = "list" ] ; then
     echo Available voices: $(echo $voicelist | sed 's/ /, /g;s/_/ /g') | $format
     echo "HANDY TIP: use \"$(basename $0) demo\" to hear all the voices"
     exit 0
   fi

   for name in $voicelist ; do
     myname=$(echo $name | sed 's/_/ /')
     echo "Voice: $myname"
     $dosay -v "$myname" "Hello! I'm $myname. This is what I sound like."
   done

   exit 0
}

usage()
{
   echo "Usage: sayit [-v voice] [-r rate] [-f file] phrase"
   echo "   or: sayit demo"
   exit 0
}

while getopts "df:r:v:" opt; do
  case $opt in
    d ) demovoices list	   ;;
    f ) input="$OPTARG"    ;;
    r )  rate="-r $OPTARG" ;;
    v ) voice="$OPTARG"    ;;
  esac
done

shift $(($OPTIND - 1))

if [ $# -eq 0 -a -z "$input" ] ; then
  $dosay "Dude! You haven't given me any parameters to work with."
  echo "Error: no parameters specified. Specify a file or phrase"
  exit 0
fi

if [ "$1" = "demo" ] ; then
  demovoices
fi

if [ ! -z "$input" ] ; then
  $dosay $rate -v "$voice" -f $input
else
  $dosay $rate -v "$voice" "$*"
fi
exit 0
```

## #94 A Smarter Image Size Analyzer
**Explicacion del codigo:** Un script que muestra la información y las dimensiones del archivo de imagen utilizando la utilidad "identify" de ImageMagick.

>Codigo a corregir:
```sh
#!/bin/bash
# imagesize--Displays image file information and dimensions using the
# identify utility from ImageMagick
for name
do
 identify -format "%f: %G with %k colors.\n" "$name"
done
exit 0

```
>Codigo corregido:
```sh
#!/bin/bash

for name
do
  identify -format "%f: %G with %k colors.\n" "$name"
done
exit 0
```

## #95 Watermarking Images
**Explicacion del codigo:** Un script que agrega marca de agua a las imagenes a traves de un texto especificado en la iamgen de entrada para despues guardar la salida como image+wm.

>Codigo a corregir:
```sh
#!/bin/bash
# watermark--Adds specified text as a watermark on the input image,
# saving the output as image+wm
wmfile="/tmp/watermark.$$.png"
fontsize="44" # Should be a starting arg
trap "$(which rm) -f $wmfile" 0 1 15 # No temp file left behind
if [ $# -ne 2 ] ; then
 echo "Usage: $(basename $0) imagefile \"watermark text\"" >&2
 exit 1
fi
if [ ! -r "$1" ] ; then
 echo "$(basename $0): Can't read input image $1" >&2
 exit 1
fi
# To start, get the dimensions of the image.
 dimensions="$(identify -format "%G" "$1")"
# Let's create the temporary watermark overlay.
 convert -size $dimensions xc:none -pointsize $fontsize -gravity south \
 -draw "fill black text 1,1 '$2' text 0,0 '$2' fill white text 2,2 '$2'" \
 $wmfile
# Now let's composite the overlay and the original file.
 suffix="$(echo $1 | rev | cut -d. -f1 | rev)"
prefix="$(echo $1 | rev | cut -d. -f2- | rev)"
ImageMagick and Working with Graphics Files 317
newfilename="$prefix+wm.$suffix"
 composite -dissolve 75% -gravity south $wmfile "$1" "$newfilename"
echo "Created new watermarked image file $newfilename."
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

wmfile="/tmp/watermark.$$.png"
fontsize="44"		

trap "`which rm` -f $wmfile" 0 1 15  

if [ $# -ne 2 ] ; then
  echo "Usage: $(basename $0) imagefile "watermark text"" >&2 ; exit 1
fi

if [ ! -r "$1" ] ; then
  echo "$(basename $0): Can't read input image $1" >&2 ; exit 1
fi

dimensions="$(identify -format "%G" "$1")"

convert -size $dimensions xc:none -pointsize $fontsize -gravity south \
  -draw "fill black text 1,1 '$2' text 0,0 '$2' fill white text 2,2 '$2'" \
  $wmfile

suffix="$(echo $1 | rev | cut -d. -f1 | rev)"
prefix="$(echo $1 | rev | cut -d. -f2- | rev)"

newfilename="$prefix+wm.$suffix"
composite -dissolve 75% -gravity south $wmfile "$1" "$newfilename"

echo "Created new watermarked image file $newfilename"

exit 0
```

## #96 Framing Images
**Explicacion del codigo:** Un script que facilita la adición de un marco gráfico alrededor un archivo de imagen, usando ImageMagick.

>Codigo a corregir:
```sh
#!/bin/bash
# frameit--Makes it easy to add a graphical frame around
# an image file, using ImageMagick
usage()
{
cat << EOF
Usage: $(basename $0) -b border -c color imagename
 or $(basename $0) -f frame -m color imagename
In the first case, specify border parameters as size x size or
percentage x percentage followed by the color desired for the
border (RGB or color name).
In the second instance, specify the frame size and offset,
followed by the matte color.
EXAMPLE USAGE:
 $(basename $0) -b 15x15 -c black imagename
 $(basename $0) -b 10%x10% -c gray imagename
 $(basename $0) -f 10x10+10+0 imagename
 $(basename $0) -f 6x6+2+2 -m tomato imagename
EOF
exit 1
}
#### MAIN CODE BLOCK
# Most of this is parsing starting arguments!
while getopts "b:c:f:m:" opt; do
 case $opt in
320 Chapter 14
 b ) border="$OPTARG"; ;;
 c ) bordercolor="$OPTARG"; ;;
 f ) frame="$OPTARG"; ;;
 m ) mattecolor="$OPTARG"; ;;
 ? ) usage; ;;
 esac
done
shift $(($OPTIND - 1)) # Eat all the parsed arguments.
if [ $# -eq 0 ] ; then # No images specified?
 usage
fi
# Did we specify a border and a frame?
if [ ! -z "$bordercolor" -a ! -z "$mattecolor" ] ; then
 echo "$0: You can't specify a color and matte color simultaneously." >&2
 exit 1
fi
if [ ! -z "$frame" -a ! -z "$border" ] ; then
 echo "$0: You can't specify a border and frame simultaneously." >&2
 exit 1
fi
if [ ! -z "$border" ] ; then
 args="-bordercolor $bordercolor -border $border"
else
 args="-mattecolor $mattecolor -frame $frame"
fi
 for name
do
 suffix="$(echo $name | rev | cut -d. -f1 | rev)"
 prefix="$(echo $name | rev | cut -d. -f2- | rev)"
 newname="$prefix+f.$suffix"
 echo "Adding a frame to image $name, saving as $newname"
 convert $name $args $newname
done
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

usage()
{
cat << EOF
Usage: $(basename $0) -b border -c color imagename
   or  $(basename $0) -f frame  -m color imagename
In the first case, specify border parameters as size x size or percentage x percentage
followed by the color desired for the border (RGB or color name).
In the second instance, specify the frame size and offset, followed by the
matte color.
EXAMPLE USAGE:
   $(basename $0) -b 15x15 -c black imagename
   $(basename $0) -b 10%x10% -c grey imagename
   $(basename $0) -f 10x10+10+0 imagename
   $(basename $0) -f 6x6+2+2 -m tomato imagename
EOF
exit 1
}

while getopts "b:c:f:m:" opt; do
  case $opt in
   b ) border="$OPTARG";                ;;
   c ) bordercolor="$OPTARG";           ;;
   f ) frame="$OPTARG";                 ;;
   m ) mattecolor="$OPTARG";            ;;
   ? ) usage;                           ;;
  esac
done
shift $(($OPTIND - 1))  

if [ $# -eq 0 ] ; then     
  usage
fi

if [ ! -z "$bordercolor" -a ! -z "$mattecolor" ] ; then
  echo "$(basename $0): You can't specify a color and matte color simultaneously." >&2
  exit 1
fi

if [ ! -z "$frame" -a ! -z "$border" ] ; then
  echo "$(basename $0): You can't specify a border and frame simultaneously." >&2
  exit 1
fi

if [ ! -z "$border" ] ; then
  args="-bordercolor $bordercolor -border $border"
else
  args="-mattecolor $mattecolor -frame $frame"
fi


for name
do
  suffix="$(echo $name | rev | cut -d. -f1 | rev)"
  prefix="$(echo $name | rev | cut -d. -f2- | rev)"
  newname="$prefix+f.$suffix"
  echo "Adding a frame to image $name, saving as $newname"
  convert $name $args $newname
done

exit 0
```

## #98 Interpreting GPS Geolocation Information
**Explicacion del codigo:** Un script que a traves de imagenes que cuentan con informacion de GPS, la convierte en una cadena que se puede enviar a Google Maps o Bing Maps(El cual ya no es muy utilizado hoy en dia por lo que recomiendo google).

>Codigo a corregir:
```sh
#!/bin/bash
# geoloc--For images that have GPS information, converts that data into
# a string that can be fed to Google Maps or Bing Maps
tempfile="/tmp/geoloc.$$"
trap "$(which rm) -f $tempfile" 0 1 15
if [ $# -eq 0 ] ; then
 echo "Usage: $(basename $0) image" >&2
 exit 1
fi
for filename
do
 identify -formatX "%[EXIF:*]" "$filename" | grep GPSL > $tempfile
Y latdeg=$(head -1 $tempfile | cut -d, -f1 | cut -d= -f2)
 latdeg=$(scriptbc -p 0 $latdeg)
 latmin=$(head -1 $tempfile | cut -d, -f2)
 latmin=$(scriptbc -p 0 $latmin)
 latsec=$(head -1 $tempfile | cut -d, -f3)
 latsec=$(scriptbc $latsec)
 latorientation=$(sed -n '2p' $tempfile | cut -d= -f2)
 longdeg=$(sed -n '3p' $tempfile | cut -d, -f1 | cut -d= -f2)
 longdeg=$(scriptbc -p 0 $longdeg)
 longmin=$(sed -n '3p' $tempfile | cut -d, -f2)
 longmin=$(scriptbc -p 0 $longmin)
 longsec=$(sed -n '3p' $tempfile | cut -d, -f3)
 longsec=$(scriptbc $longsec)
 longorientation=$(sed -n '4p' $tempfile | cut -d= -f2)
Z echon "Coords: $latdeg ${latmin}' ${latsec}\" $latorientation, "
 echo "$longdeg ${longmin}' ${longsec}\" $longorientation"
ImageMagick and Working with Graphics Files 327
done
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

tempfile="/tmp/geoloc.$$"

trap "`which rm` -f $tempfile" 0 1 15

if [ $# -eq 0 ] ; then
  echo "Usage: $(basename $0) image" >&2 ; exit 1
fi

for filename
do
  identify -format "%[EXIF:*]" "$filename" | grep GPSL > $tempfile

  latdeg=$(head -1 $tempfile | cut -d, -f1 | cut -d= -f2)
  latdeg=$(scriptbc -p 0 $latdeg)
  latmin=$(head -1 $tempfile | cut -d, -f2)
  latmin=$(scriptbc -p 0 $latmin)
  latsec=$(head -1 $tempfile | cut -d, -f3)
  latsec=$(scriptbc $latsec)
  latorientation=$(sed -n '2p' $tempfile | cut -d= -f2)

  longdeg=$(sed -n '3p' $tempfile | cut -d, -f1 | cut -d= -f2)
  longdeg=$(scriptbc -p 0 $longdeg)
  longmin=$(sed -n '3p' $tempfile | cut -d, -f2)
  longmin=$(scriptbc -p 0 $longmin)
  longsec=$(sed -n '3p' $tempfile | cut -d, -f3)
  longsec=$(scriptbc $longsec)
  longorientation=$(sed -n '4p' $tempfile | cut -d= -f2)

  /bin/echo -n "Coords: $latdeg ${latmin}' ${latsec}\" $latorientation, "
  echo "$longdeg ${longmin}' ${longsec}\" $longorientation"

done

exit 0
```

## #99 Finding the Day of a Specific Date in the Past
**Explicacion del codigo:** Un script que permite Encontrar el día de una fecha específica en el pasado e informa qué día de la semana fue.

>Codigo a corregir:
```sh
#!/bin/bash
# dayinpast--Given a date, reports what day of the week it was
if [ $# -ne 3 ] ; then
 echo "Usage: $(basename $0) mon day year" >&2
 echo " with just numerical values (ex: 7 7 1776)" >&2
 exit 1
fi
date --version > /dev/null 2>&1 # Discard error, if any.
baddate="$?" # Just look at return code.
if [ ! $baddate ] ; then
 date -d $1/$2/$3 +"That was a %A."
else
 if [ $2 -lt 10 ] ; then
 pattern=" $2[^0-9]"
 else
 pattern="$2[^0-9]"
 fi
 dayofweek="$(ncal $1 $3 | grep "$pattern" | cut -c1-2)"
Days and Dates 331
 case $dayofweek in
 Su ) echo "That was a Sunday."; ;;
 Mo ) echo "That was a Monday."; ;;
 Tu ) echo "That was a Tuesday."; ;;
 We ) echo "That was a Wednesday."; ;;
 Th ) echo "That was a Thursday."; ;;
 Fr ) echo "That was a Friday."; ;;
 Sa ) echo "That was a Saturday."; ;;
 esac
fi
exit 0
```
>Codigo corregido:
```sh
#!/bin/bash

if [ $# -ne 3 ] ; then
  echo "Usage: $(basename $0) mon day year" >&2
  echo "  with just numerical values (ex: 7 7 1776)" >&2
  exit 1
fi

date --version > /dev/null 2>&1 	
baddate="$?"				

if [ ! $baddate ] ; then
  date -d $1/$2/$3 +"That was a %A."
else

  if [ $2 -lt 10 ] ; then
    pattern=" $2[^0-9]"
  else
    pattern="$2[^0-9]"
  fi

  dayofweek="$(ncal $1 $3 | grep "$pattern" | cut -c1-2)"

  case $dayofweek in 
    Su ) echo "That was a Sunday"; 	;;
    Mo ) echo "That was a Monday"; 	;;
    Tu ) echo "That was a Tuesday"; 	;;
    We ) echo "That was a Wednesday"; 	;;
    Th ) echo "That was a Thursday"; 	;;
    Fr ) echo "That was a Friday"; 	;;
    Sa ) echo "That was a Saturday"; 	;;
  esac
fi
exit 0
```