# $Id: Makefile,v 1.13 2015-02-19 16:48:00-08 - - $

MKFILE      = Makefile
DEPFILE     = ${MKFILE}.dep
NOINCL      = ci clean spotless
NEEDINCL    = ${filter ${NOINCL}, ${MAKECMDGOALS}}
GMAKE       = ${MAKE} --no-print-directory
COMPILECPP  = g++ -g -O0 -Wall -Wextra -std=gnu++11
MAKEDEPCPP  = g++ -MM -std=gnu++11

MODULES     = debug graphics interp rgbcolor shape util
CPPHEADER   = ${MODULES:=.h}
CPPSOURCE   = ${MODULES:=.cpp} main.cpp
TEMPLATES   = util.tcc
GENFILES    = colors.cppgen
OTHERS      = ${MKFILE} README mk-colors.perl
ALLSOURCES  = ${CPPHEADER} ${TEMPLATES} ${CPPSOURCE} ${OTHERS}
EXECBIN     = gdraw
OBJECTS     = ${CPPSOURCE:.cpp=.o}
LINKLIBS    = -lGL -lGLU -lglut -lm

CLASS       = cmps109-wm.w15
PROJECT     = asg4

all : ${EXECBIN}
	- checksource ${ALLSOURCES}

${EXECBIN} : ${OBJECTS}
	${COMPILECPP} -o $@ ${OBJECTS} ${LINKLIBS}

%.o : %.cpp
	${COMPILECPP} -c $<

colors.cppgen: mk-colors.perl
	mk-colors.perl >colors.cppgen

ci : ${ALLSOURCES}
	- checksource ${ALLSOURCES}
	cid + ${ALLSOURCES}

clean :
	- rm ${OBJECTS} ${DEPFILE} ${GENFILES}

spotless : clean
	- rm ${EXECBIN} 


submit : ${ALLSOURCES}
	- checksource ${ALLSOURCES}
	submit ${CLASS} ${PROJECT} ${ALLSOURCES}
	cp ${ALLSOURCES} ../testing

dep : ${CPPSOURCE} ${CPPHEADER} ${GENFILES}
	@ echo "# ${DEPFILE} created `LC_TIME=C date`" >${DEPFILE}
	${MAKEDEPCPP} ${CPPSOURCE} >>${DEPFILE}

${DEPFILE} :
	@ touch ${DEPFILE}
	${GMAKE} dep

again :
	${GMAKE} spotless dep ci all lis

ifeq (${NEEDINCL}, )
include ${DEPFILE}
endif

