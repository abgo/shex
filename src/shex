#!/bin/bash
# Self-Extracting tarball creator for GNU/Linux
# Return codes:
#        1 - Usage error
#        2 - Folder not specified
#        3 - Wrong argument values
#        4 - Permission error

usage() {
   echo "Usage: $0 [-s script] [-o output] target";
   echo "   target - the target forlder to be archived";
   echo "   script - the script inside the target folder to be ran after being extracted"
   echo "   output - the output file to be generated"
}

SCRIPT=
DIR=
OUT=

while getopts ":s:o:" OPTION
do
   case $OPTION in
      s)
         SCRIPT=$OPTARG;  
      ;;
      o)
         OUT=$OPTARG;
      ;;
      ?)
         usage;
         exit 1;
      ;;
   esac
done
shift $(($OPTIND -1))

DIR=$1

if [ -z $DIR ]
then
   echo "Please specify the directory to be archived.";
   exit 2;
else
   if [ ! -d $DIR ]
   then
      echo "$DIR: No such directory";
      exit 3;
   fi
fi

if [ ! -z $SCRIPT ]
then
   if [ ! -f $DIR/$SCRIPT ]
   then
      echo "$DIR/$SCRIPT: No such file";
      exit 3;
   fi

   chmod +x $DIR/$SCRIPT;

   if [ $? -eq 1 ]
   then
      echo "No permission for chmoding";
      exit 4;
   fi

   SCRIPT="&& ./$DIR/$SCRIPT";
fi

if [ -z $OUT ]
then
   OUT="$DIR.sh"
fi

# Prepare tarball

echo "#!/bin/bash" > $OUT
echo "sed -e '1,/^exit$/d' \"\$0\" | tar xzf - $SCRIPT" >> $OUT
chmod +x $OUT
echo "exit" >> $OUT

tar cvf - $DIR >> $OUT

chmod +x $OUT
