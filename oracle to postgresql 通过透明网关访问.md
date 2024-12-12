# oracle to postgresql 通过透明网关访问

安装包：
yum install -y  postgresql-devel  unixODBC-devel

前置条件：在linux机器上部署，19c数据库，会自动安装透明网关。

[psqlodbc下载](https://www.postgresql.org/ftp/odbc/versions.old/src/)

查看odbc驱动信息
```
[root@localhost psqlodbc-16.00.0000]# odbcinst -j
unixODBC 2.3.7
DRIVERS............: /etc/odbcinst.ini
SYSTEM DATA SOURCES: /etc/odbc.ini
FILE DATA SOURCES..: /etc/ODBCDataSources
USER DATA SOURCES..: /root/.odbc.ini
SQLULEN Size.......: 8
SQLLEN Size........: 8
SQLSETPOSIROW Size.: 8
```

编译安装psqlodbc

```
[root@localhost psqlodbc-16.00.0000]# ./configure  CPPFLAGS="-DSQLCOLATTRIBUTE_SQLLEN"  --prefix=/usr --libdir=/usr/lib64 --sysconfdir=/etc
[root@localhost psqlodbc-16.00.0000]#  make && make

```

完整结果如下显示 ：

```
[root@localhost psqlodbc-16.00.0000]# ./configure  CPPFLAGS="-DSQLCOLATTRIBUTE_SQLLEN"  --prefix=/usr --libdir=/usr/lib64 --sysconfdir=/etc
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /usr/bin/mkdir -p
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking whether to enable maintainer-specific portions of Makefiles... no
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking whether gcc understands -c and -o together... yes
checking for style of include used by make... GNU
checking dependency style of gcc... gcc3
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking size of long... 8
checking size of long int... 8
checking size of void *... 8
checking for long long... yes
checking for signed char... yes
checking for ssize_t... yes
checking size of bool... 0
checking for size_t... yes
checking -Wall is a valid compile option... yes
checking for odbc_config... /usr/bin/odbc_config
configure: using /usr/include /usr/lib64
configure: using -L/usr/lib64 -lodbc for regression test
checking last argument to SQLColAttribute is SQLLEN *... yes
checking for pg_config... /usr/bin/pg_config
checking for prove... no
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking how to print strings... printf
checking for a sed that does not truncate output... /usr/bin/sed
checking for fgrep... /usr/bin/grep -F
checking for ld used by gcc... /usr/bin/ld
checking if the linker (/usr/bin/ld) is GNU ld... yes
checking for BSD- or MS-compatible name lister (nm)... /usr/bin/nm -B
checking the name lister (/usr/bin/nm -B) interface... BSD nm
checking whether ln -s works... yes
checking the maximum length of command line arguments... 1572864
checking whether the shell understands some XSI constructs... yes
checking whether the shell understands "+="... yes
checking how to convert x86_64-unknown-linux-gnu file names to x86_64-unknown-linux-gnu format... func_convert_file_noop
checking how to convert x86_64-unknown-linux-gnu file names to toolchain format... func_convert_file_noop
checking for /usr/bin/ld option to reload object files... -r
checking for objdump... objdump
checking how to recognize dependent libraries... pass_all
checking for dlltool... dlltool
checking how to associate runtime and link libraries... printf %s\n
checking for ar... ar
checking for archiver @FILE support... @
checking for strip... strip
checking for ranlib... ranlib
checking command to parse /usr/bin/nm -B output from gcc object... ok
checking for sysroot... no
checking for mt... no
checking if : is a manifest tool... no
checking for dlfcn.h... yes
checking for objdir... .libs
checking if gcc supports -fno-rtti -fno-exceptions... no
checking for gcc option to produce PIC... -fPIC -DPIC
checking if gcc PIC flag -fPIC -DPIC works... yes
checking if gcc static flag -static works... no
checking if gcc supports -c -o file.o... yes
checking if gcc supports -c -o file.o... (cached) yes
checking whether the gcc linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
checking whether -lc should be explicitly linked in... no
checking dynamic linker characteristics... GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
checking for shl_load... no
checking for shl_load in -ldld... no
checking for dlopen... no
checking for dlopen in -ldl... yes
checking whether a program can dlopen itself... yes
checking whether a statically linked program can dlopen itself... yes
checking whether stripping libraries is possible... yes
checking if libtool supports shared libraries... yes
checking whether to build shared libraries... yes
checking whether to build static libraries... no
checking for library containing SQLGetPrivateProfileString... -lodbcinst
checking for pthread_create in -lpthreads... no
checking for pthread_create in -lpthread... yes
checking for PQsetSingleRowMode in -lpq... yes
checking locale.h usability... yes
checking locale.h presence... yes
checking for locale.h... yes
checking sys/time.h usability... yes
checking sys/time.h presence... yes
checking for sys/time.h... yes
checking uchar.h usability... yes
checking uchar.h presence... yes
checking for uchar.h... yes
checking libpq-fe.h usability... yes
checking libpq-fe.h presence... yes
checking for libpq-fe.h... yes
checking whether time.h and sys/time.h may both be included... yes
checking for stdbool.h that conforms to C99... yes
checking for _Bool... yes
checking whether struct tm is in sys/time.h or time.h... time.h
checking for an ANSI C-conforming const... yes
checking whether strerror_r is declared... yes
checking for strerror_r... yes
checking whether strerror_r returns char *... no
checking for strtoul... yes
checking for strtoll... yes
checking for strlcat... no
checking for mbstowcs... yes
checking for wcstombs... yes
checking for mbrtoc16... yes
checking for c16rtomb... yes
checking for PQsslInUse... yes
checking for localtime_r... yes
checking for strtok_r... yes
checking for pthread_mutexattr_settype... yes
checking that generated files are newer than configure... done
configure: creating ./config.status
config.status: creating Makefile
config.status: creating test/Makefile
config.status: creating config.h
config.status: executing depfiles commands
config.status: executing libtool commands
[root@localhost psqlodbc-16.00.0000]# make
make  all-am
make[1]: 进入目录“/mnt/psqlodbc-16.00.0000”
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-info.lo -MD -MP -MF .deps/psqlodbcw_la-info.Tpo -c -o psqlodbcw_la-info.lo `test -f 'info.c' || echo './'`info.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-info.lo -MD -MP -MF .deps/psqlodbcw_la-info.Tpo -c info.c  -fPIC -DPIC -o .libs/psqlodbcw_la-info.o
mv -f .deps/psqlodbcw_la-info.Tpo .deps/psqlodbcw_la-info.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-bind.lo -MD -MP -MF .deps/psqlodbcw_la-bind.Tpo -c -o psqlodbcw_la-bind.lo `test -f 'bind.c' || echo './'`bind.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-bind.lo -MD -MP -MF .deps/psqlodbcw_la-bind.Tpo -c bind.c  -fPIC -DPIC -o .libs/psqlodbcw_la-bind.o
mv -f .deps/psqlodbcw_la-bind.Tpo .deps/psqlodbcw_la-bind.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-columninfo.lo -MD -MP -MF .deps/psqlodbcw_la-columninfo.Tpo -c -o psqlodbcw_la-columninfo.lo `test -f 'columninfo.c' || echo './'`columninfo.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-columninfo.lo -MD -MP -MF .deps/psqlodbcw_la-columninfo.Tpo -c columninfo.c  -fPIC -DPIC -o .libs/psqlodbcw_la-columninfo.o
mv -f .deps/psqlodbcw_la-columninfo.Tpo .deps/psqlodbcw_la-columninfo.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-connection.lo -MD -MP -MF .deps/psqlodbcw_la-connection.Tpo -c -o psqlodbcw_la-connection.lo `test -f 'connection.c' || echo './'`connection.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-connection.lo -MD -MP -MF .deps/psqlodbcw_la-connection.Tpo -c connection.c  -fPIC -DPIC -o .libs/psqlodbcw_la-connection.o
mv -f .deps/psqlodbcw_la-connection.Tpo .deps/psqlodbcw_la-connection.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-convert.lo -MD -MP -MF .deps/psqlodbcw_la-convert.Tpo -c -o psqlodbcw_la-convert.lo `test -f 'convert.c' || echo './'`convert.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-convert.lo -MD -MP -MF .deps/psqlodbcw_la-convert.Tpo -c convert.c  -fPIC -DPIC -o .libs/psqlodbcw_la-convert.o
mv -f .deps/psqlodbcw_la-convert.Tpo .deps/psqlodbcw_la-convert.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-drvconn.lo -MD -MP -MF .deps/psqlodbcw_la-drvconn.Tpo -c -o psqlodbcw_la-drvconn.lo `test -f 'drvconn.c' || echo './'`drvconn.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-drvconn.lo -MD -MP -MF .deps/psqlodbcw_la-drvconn.Tpo -c drvconn.c  -fPIC -DPIC -o .libs/psqlodbcw_la-drvconn.o
mv -f .deps/psqlodbcw_la-drvconn.Tpo .deps/psqlodbcw_la-drvconn.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-environ.lo -MD -MP -MF .deps/psqlodbcw_la-environ.Tpo -c -o psqlodbcw_la-environ.lo `test -f 'environ.c' || echo './'`environ.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-environ.lo -MD -MP -MF .deps/psqlodbcw_la-environ.Tpo -c environ.c  -fPIC -DPIC -o .libs/psqlodbcw_la-environ.o
mv -f .deps/psqlodbcw_la-environ.Tpo .deps/psqlodbcw_la-environ.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-execute.lo -MD -MP -MF .deps/psqlodbcw_la-execute.Tpo -c -o psqlodbcw_la-execute.lo `test -f 'execute.c' || echo './'`execute.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-execute.lo -MD -MP -MF .deps/psqlodbcw_la-execute.Tpo -c execute.c  -fPIC -DPIC -o .libs/psqlodbcw_la-execute.o
mv -f .deps/psqlodbcw_la-execute.Tpo .deps/psqlodbcw_la-execute.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-lobj.lo -MD -MP -MF .deps/psqlodbcw_la-lobj.Tpo -c -o psqlodbcw_la-lobj.lo `test -f 'lobj.c' || echo './'`lobj.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-lobj.lo -MD -MP -MF .deps/psqlodbcw_la-lobj.Tpo -c lobj.c  -fPIC -DPIC -o .libs/psqlodbcw_la-lobj.o
mv -f .deps/psqlodbcw_la-lobj.Tpo .deps/psqlodbcw_la-lobj.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-misc.lo -MD -MP -MF .deps/psqlodbcw_la-misc.Tpo -c -o psqlodbcw_la-misc.lo `test -f 'misc.c' || echo './'`misc.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-misc.lo -MD -MP -MF .deps/psqlodbcw_la-misc.Tpo -c misc.c  -fPIC -DPIC -o .libs/psqlodbcw_la-misc.o
mv -f .deps/psqlodbcw_la-misc.Tpo .deps/psqlodbcw_la-misc.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-options.lo -MD -MP -MF .deps/psqlodbcw_la-options.Tpo -c -o psqlodbcw_la-options.lo `test -f 'options.c' || echo './'`options.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-options.lo -MD -MP -MF .deps/psqlodbcw_la-options.Tpo -c options.c  -fPIC -DPIC -o .libs/psqlodbcw_la-options.o
mv -f .deps/psqlodbcw_la-options.Tpo .deps/psqlodbcw_la-options.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-pgtypes.lo -MD -MP -MF .deps/psqlodbcw_la-pgtypes.Tpo -c -o psqlodbcw_la-pgtypes.lo `test -f 'pgtypes.c' || echo './'`pgtypes.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-pgtypes.lo -MD -MP -MF .deps/psqlodbcw_la-pgtypes.Tpo -c pgtypes.c  -fPIC -DPIC -o .libs/psqlodbcw_la-pgtypes.o
mv -f .deps/psqlodbcw_la-pgtypes.Tpo .deps/psqlodbcw_la-pgtypes.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-psqlodbc.lo -MD -MP -MF .deps/psqlodbcw_la-psqlodbc.Tpo -c -o psqlodbcw_la-psqlodbc.lo `test -f 'psqlodbc.c' || echo './'`psqlodbc.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-psqlodbc.lo -MD -MP -MF .deps/psqlodbcw_la-psqlodbc.Tpo -c psqlodbc.c  -fPIC -DPIC -o .libs/psqlodbcw_la-psqlodbc.o
mv -f .deps/psqlodbcw_la-psqlodbc.Tpo .deps/psqlodbcw_la-psqlodbc.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-qresult.lo -MD -MP -MF .deps/psqlodbcw_la-qresult.Tpo -c -o psqlodbcw_la-qresult.lo `test -f 'qresult.c' || echo './'`qresult.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-qresult.lo -MD -MP -MF .deps/psqlodbcw_la-qresult.Tpo -c qresult.c  -fPIC -DPIC -o .libs/psqlodbcw_la-qresult.o
mv -f .deps/psqlodbcw_la-qresult.Tpo .deps/psqlodbcw_la-qresult.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-results.lo -MD -MP -MF .deps/psqlodbcw_la-results.Tpo -c -o psqlodbcw_la-results.lo `test -f 'results.c' || echo './'`results.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-results.lo -MD -MP -MF .deps/psqlodbcw_la-results.Tpo -c results.c  -fPIC -DPIC -o .libs/psqlodbcw_la-results.o
mv -f .deps/psqlodbcw_la-results.Tpo .deps/psqlodbcw_la-results.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-parse.lo -MD -MP -MF .deps/psqlodbcw_la-parse.Tpo -c -o psqlodbcw_la-parse.lo `test -f 'parse.c' || echo './'`parse.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-parse.lo -MD -MP -MF .deps/psqlodbcw_la-parse.Tpo -c parse.c  -fPIC -DPIC -o .libs/psqlodbcw_la-parse.o
mv -f .deps/psqlodbcw_la-parse.Tpo .deps/psqlodbcw_la-parse.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-statement.lo -MD -MP -MF .deps/psqlodbcw_la-statement.Tpo -c -o psqlodbcw_la-statement.lo `test -f 'statement.c' || echo './'`statement.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-statement.lo -MD -MP -MF .deps/psqlodbcw_la-statement.Tpo -c statement.c  -fPIC -DPIC -o .libs/psqlodbcw_la-statement.o
mv -f .deps/psqlodbcw_la-statement.Tpo .deps/psqlodbcw_la-statement.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-tuple.lo -MD -MP -MF .deps/psqlodbcw_la-tuple.Tpo -c -o psqlodbcw_la-tuple.lo `test -f 'tuple.c' || echo './'`tuple.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-tuple.lo -MD -MP -MF .deps/psqlodbcw_la-tuple.Tpo -c tuple.c  -fPIC -DPIC -o .libs/psqlodbcw_la-tuple.o
mv -f .deps/psqlodbcw_la-tuple.Tpo .deps/psqlodbcw_la-tuple.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-dlg_specific.lo -MD -MP -MF .deps/psqlodbcw_la-dlg_specific.Tpo -c -o psqlodbcw_la-dlg_specific.lo `test -f 'dlg_specific.c' || echo './'`dlg_specific.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-dlg_specific.lo -MD -MP -MF .deps/psqlodbcw_la-dlg_specific.Tpo -c dlg_specific.c  -fPIC -DPIC -o .libs/psqlodbcw_la-dlg_specific.o
mv -f .deps/psqlodbcw_la-dlg_specific.Tpo .deps/psqlodbcw_la-dlg_specific.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-multibyte.lo -MD -MP -MF .deps/psqlodbcw_la-multibyte.Tpo -c -o psqlodbcw_la-multibyte.lo `test -f 'multibyte.c' || echo './'`multibyte.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-multibyte.lo -MD -MP -MF .deps/psqlodbcw_la-multibyte.Tpo -c multibyte.c  -fPIC -DPIC -o .libs/psqlodbcw_la-multibyte.o
mv -f .deps/psqlodbcw_la-multibyte.Tpo .deps/psqlodbcw_la-multibyte.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-odbcapi.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapi.Tpo -c -o psqlodbcw_la-odbcapi.lo `test -f 'odbcapi.c' || echo './'`odbcapi.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-odbcapi.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapi.Tpo -c odbcapi.c  -fPIC -DPIC -o .libs/psqlodbcw_la-odbcapi.o
mv -f .deps/psqlodbcw_la-odbcapi.Tpo .deps/psqlodbcw_la-odbcapi.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-descriptor.lo -MD -MP -MF .deps/psqlodbcw_la-descriptor.Tpo -c -o psqlodbcw_la-descriptor.lo `test -f 'descriptor.c' || echo './'`descriptor.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-descriptor.lo -MD -MP -MF .deps/psqlodbcw_la-descriptor.Tpo -c descriptor.c  -fPIC -DPIC -o .libs/psqlodbcw_la-descriptor.o
mv -f .deps/psqlodbcw_la-descriptor.Tpo .deps/psqlodbcw_la-descriptor.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-odbcapi30.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapi30.Tpo -c -o psqlodbcw_la-odbcapi30.lo `test -f 'odbcapi30.c' || echo './'`odbcapi30.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-odbcapi30.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapi30.Tpo -c odbcapi30.c  -fPIC -DPIC -o .libs/psqlodbcw_la-odbcapi30.o
mv -f .deps/psqlodbcw_la-odbcapi30.Tpo .deps/psqlodbcw_la-odbcapi30.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-pgapi30.lo -MD -MP -MF .deps/psqlodbcw_la-pgapi30.Tpo -c -o psqlodbcw_la-pgapi30.lo `test -f 'pgapi30.c' || echo './'`pgapi30.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-pgapi30.lo -MD -MP -MF .deps/psqlodbcw_la-pgapi30.Tpo -c pgapi30.c  -fPIC -DPIC -o .libs/psqlodbcw_la-pgapi30.o
mv -f .deps/psqlodbcw_la-pgapi30.Tpo .deps/psqlodbcw_la-pgapi30.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-mylog.lo -MD -MP -MF .deps/psqlodbcw_la-mylog.Tpo -c -o psqlodbcw_la-mylog.lo `test -f 'mylog.c' || echo './'`mylog.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-mylog.lo -MD -MP -MF .deps/psqlodbcw_la-mylog.Tpo -c mylog.c  -fPIC -DPIC -o .libs/psqlodbcw_la-mylog.o
mv -f .deps/psqlodbcw_la-mylog.Tpo .deps/psqlodbcw_la-mylog.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-odbcapi30w.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapi30w.Tpo -c -o psqlodbcw_la-odbcapi30w.lo `test -f 'odbcapi30w.c' || echo './'`odbcapi30w.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-odbcapi30w.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapi30w.Tpo -c odbcapi30w.c  -fPIC -DPIC -o .libs/psqlodbcw_la-odbcapi30w.o
mv -f .deps/psqlodbcw_la-odbcapi30w.Tpo .deps/psqlodbcw_la-odbcapi30w.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-odbcapiw.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapiw.Tpo -c -o psqlodbcw_la-odbcapiw.lo `test -f 'odbcapiw.c' || echo './'`odbcapiw.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-odbcapiw.lo -MD -MP -MF .deps/psqlodbcw_la-odbcapiw.Tpo -c odbcapiw.c  -fPIC -DPIC -o .libs/psqlodbcw_la-odbcapiw.o
mv -f .deps/psqlodbcw_la-odbcapiw.Tpo .deps/psqlodbcw_la-odbcapiw.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT  -g -O2 -Wall -MT psqlodbcw_la-win_unicode.lo -MD -MP -MF .deps/psqlodbcw_la-win_unicode.Tpo -c -o psqlodbcw_la-win_unicode.lo `test -f 'win_unicode.c' || echo './'`win_unicode.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -DUNICODE_SUPPORT -g -O2 -Wall -MT psqlodbcw_la-win_unicode.lo -MD -MP -MF .deps/psqlodbcw_la-win_unicode.Tpo -c win_unicode.c  -fPIC -DPIC -o .libs/psqlodbcw_la-win_unicode.o
mv -f .deps/psqlodbcw_la-win_unicode.Tpo .deps/psqlodbcw_la-win_unicode.Plo
/bin/sh ./libtool  --tag=CC   --mode=link gcc -DUNICODE_SUPPORT  -g -O2 -Wall -module -no-undefined -avoid-version -export-symbols-regex '^SQL' -L/usr/lib64 -L/usr/lib64 -o psqlodbcw.la -rpath /usr/lib64 psqlodbcw_la-info.lo psqlodbcw_la-bind.lo psqlodbcw_la-columninfo.lo psqlodbcw_la-connection.lo psqlodbcw_la-convert.lo psqlodbcw_la-drvconn.lo psqlodbcw_la-environ.lo psqlodbcw_la-execute.lo psqlodbcw_la-lobj.lo psqlodbcw_la-misc.lo psqlodbcw_la-options.lo psqlodbcw_la-pgtypes.lo psqlodbcw_la-psqlodbc.lo psqlodbcw_la-qresult.lo psqlodbcw_la-results.lo psqlodbcw_la-parse.lo psqlodbcw_la-statement.lo psqlodbcw_la-tuple.lo psqlodbcw_la-dlg_specific.lo psqlodbcw_la-multibyte.lo psqlodbcw_la-odbcapi.lo psqlodbcw_la-descriptor.lo psqlodbcw_la-odbcapi30.lo psqlodbcw_la-pgapi30.lo psqlodbcw_la-mylog.lo psqlodbcw_la-odbcapi30w.lo psqlodbcw_la-odbcapiw.lo psqlodbcw_la-win_unicode.lo  -lpq -lpthread -lodbcinst 
libtool: link: /usr/bin/nm -B  .libs/psqlodbcw_la-info.o .libs/psqlodbcw_la-bind.o .libs/psqlodbcw_la-columninfo.o .libs/psqlodbcw_la-connection.o .libs/psqlodbcw_la-convert.o .libs/psqlodbcw_la-drvconn.o .libs/psqlodbcw_la-environ.o .libs/psqlodbcw_la-execute.o .libs/psqlodbcw_la-lobj.o .libs/psqlodbcw_la-misc.o .libs/psqlodbcw_la-options.o .libs/psqlodbcw_la-pgtypes.o .libs/psqlodbcw_la-psqlodbc.o .libs/psqlodbcw_la-qresult.o .libs/psqlodbcw_la-results.o .libs/psqlodbcw_la-parse.o .libs/psqlodbcw_la-statement.o .libs/psqlodbcw_la-tuple.o .libs/psqlodbcw_la-dlg_specific.o .libs/psqlodbcw_la-multibyte.o .libs/psqlodbcw_la-odbcapi.o .libs/psqlodbcw_la-descriptor.o .libs/psqlodbcw_la-odbcapi30.o .libs/psqlodbcw_la-pgapi30.o .libs/psqlodbcw_la-mylog.o .libs/psqlodbcw_la-odbcapi30w.o .libs/psqlodbcw_la-odbcapiw.o .libs/psqlodbcw_la-win_unicode.o   | sed -n -e 's/^.*[	 ]\([ABCDGIRSTW][ABCDGIRSTW]*\)[	 ][	 ]*\([_A-Za-z][_A-Za-z0-9]*\)$/\1 \2 \2/p' | sed '/ __gnu_lto/d' | /usr/bin/sed 's/.* //' | sort | uniq > .libs/psqlodbcw.exp
libtool: link: /usr/bin/grep -E -e "^SQL" ".libs/psqlodbcw.exp" > ".libs/psqlodbcw.expT"
libtool: link: mv -f ".libs/psqlodbcw.expT" ".libs/psqlodbcw.exp"
libtool: link: echo "{ global:" > .libs/psqlodbcw.ver
libtool: link:  cat .libs/psqlodbcw.exp | sed -e "s/\(.*\)/\1;/" >> .libs/psqlodbcw.ver
libtool: link:  echo "local: *; };" >> .libs/psqlodbcw.ver
libtool: link:  gcc -shared  -fPIC -DPIC  .libs/psqlodbcw_la-info.o .libs/psqlodbcw_la-bind.o .libs/psqlodbcw_la-columninfo.o .libs/psqlodbcw_la-connection.o .libs/psqlodbcw_la-convert.o .libs/psqlodbcw_la-drvconn.o .libs/psqlodbcw_la-environ.o .libs/psqlodbcw_la-execute.o .libs/psqlodbcw_la-lobj.o .libs/psqlodbcw_la-misc.o .libs/psqlodbcw_la-options.o .libs/psqlodbcw_la-pgtypes.o .libs/psqlodbcw_la-psqlodbc.o .libs/psqlodbcw_la-qresult.o .libs/psqlodbcw_la-results.o .libs/psqlodbcw_la-parse.o .libs/psqlodbcw_la-statement.o .libs/psqlodbcw_la-tuple.o .libs/psqlodbcw_la-dlg_specific.o .libs/psqlodbcw_la-multibyte.o .libs/psqlodbcw_la-odbcapi.o .libs/psqlodbcw_la-descriptor.o .libs/psqlodbcw_la-odbcapi30.o .libs/psqlodbcw_la-pgapi30.o .libs/psqlodbcw_la-mylog.o .libs/psqlodbcw_la-odbcapi30w.o .libs/psqlodbcw_la-odbcapiw.o .libs/psqlodbcw_la-win_unicode.o   -L/usr/lib64 -lpq -lpthread -lodbcinst  -O2   -Wl,-soname -Wl,psqlodbcw.so -Wl,-version-script -Wl,.libs/psqlodbcw.ver -o .libs/psqlodbcw.so
libtool: link: ( cd ".libs" && rm -f "psqlodbcw.la" && ln -s "../psqlodbcw.la" "psqlodbcw.la" )
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT info.lo -MD -MP -MF .deps/info.Tpo -c -o info.lo info.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT info.lo -MD -MP -MF .deps/info.Tpo -c info.c  -fPIC -DPIC -o .libs/info.o
mv -f .deps/info.Tpo .deps/info.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT bind.lo -MD -MP -MF .deps/bind.Tpo -c -o bind.lo bind.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT bind.lo -MD -MP -MF .deps/bind.Tpo -c bind.c  -fPIC -DPIC -o .libs/bind.o
mv -f .deps/bind.Tpo .deps/bind.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT columninfo.lo -MD -MP -MF .deps/columninfo.Tpo -c -o columninfo.lo columninfo.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT columninfo.lo -MD -MP -MF .deps/columninfo.Tpo -c columninfo.c  -fPIC -DPIC -o .libs/columninfo.o
mv -f .deps/columninfo.Tpo .deps/columninfo.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT connection.lo -MD -MP -MF .deps/connection.Tpo -c -o connection.lo connection.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT connection.lo -MD -MP -MF .deps/connection.Tpo -c connection.c  -fPIC -DPIC -o .libs/connection.o
mv -f .deps/connection.Tpo .deps/connection.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT convert.lo -MD -MP -MF .deps/convert.Tpo -c -o convert.lo convert.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT convert.lo -MD -MP -MF .deps/convert.Tpo -c convert.c  -fPIC -DPIC -o .libs/convert.o
mv -f .deps/convert.Tpo .deps/convert.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT drvconn.lo -MD -MP -MF .deps/drvconn.Tpo -c -o drvconn.lo drvconn.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT drvconn.lo -MD -MP -MF .deps/drvconn.Tpo -c drvconn.c  -fPIC -DPIC -o .libs/drvconn.o
mv -f .deps/drvconn.Tpo .deps/drvconn.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT environ.lo -MD -MP -MF .deps/environ.Tpo -c -o environ.lo environ.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT environ.lo -MD -MP -MF .deps/environ.Tpo -c environ.c  -fPIC -DPIC -o .libs/environ.o
mv -f .deps/environ.Tpo .deps/environ.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT execute.lo -MD -MP -MF .deps/execute.Tpo -c -o execute.lo execute.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT execute.lo -MD -MP -MF .deps/execute.Tpo -c execute.c  -fPIC -DPIC -o .libs/execute.o
mv -f .deps/execute.Tpo .deps/execute.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT lobj.lo -MD -MP -MF .deps/lobj.Tpo -c -o lobj.lo lobj.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT lobj.lo -MD -MP -MF .deps/lobj.Tpo -c lobj.c  -fPIC -DPIC -o .libs/lobj.o
mv -f .deps/lobj.Tpo .deps/lobj.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT misc.lo -MD -MP -MF .deps/misc.Tpo -c -o misc.lo misc.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT misc.lo -MD -MP -MF .deps/misc.Tpo -c misc.c  -fPIC -DPIC -o .libs/misc.o
mv -f .deps/misc.Tpo .deps/misc.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT options.lo -MD -MP -MF .deps/options.Tpo -c -o options.lo options.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT options.lo -MD -MP -MF .deps/options.Tpo -c options.c  -fPIC -DPIC -o .libs/options.o
mv -f .deps/options.Tpo .deps/options.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT pgtypes.lo -MD -MP -MF .deps/pgtypes.Tpo -c -o pgtypes.lo pgtypes.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT pgtypes.lo -MD -MP -MF .deps/pgtypes.Tpo -c pgtypes.c  -fPIC -DPIC -o .libs/pgtypes.o
mv -f .deps/pgtypes.Tpo .deps/pgtypes.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT psqlodbc.lo -MD -MP -MF .deps/psqlodbc.Tpo -c -o psqlodbc.lo psqlodbc.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT psqlodbc.lo -MD -MP -MF .deps/psqlodbc.Tpo -c psqlodbc.c  -fPIC -DPIC -o .libs/psqlodbc.o
mv -f .deps/psqlodbc.Tpo .deps/psqlodbc.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT qresult.lo -MD -MP -MF .deps/qresult.Tpo -c -o qresult.lo qresult.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT qresult.lo -MD -MP -MF .deps/qresult.Tpo -c qresult.c  -fPIC -DPIC -o .libs/qresult.o
mv -f .deps/qresult.Tpo .deps/qresult.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT results.lo -MD -MP -MF .deps/results.Tpo -c -o results.lo results.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT results.lo -MD -MP -MF .deps/results.Tpo -c results.c  -fPIC -DPIC -o .libs/results.o
mv -f .deps/results.Tpo .deps/results.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT parse.lo -MD -MP -MF .deps/parse.Tpo -c -o parse.lo parse.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT parse.lo -MD -MP -MF .deps/parse.Tpo -c parse.c  -fPIC -DPIC -o .libs/parse.o
mv -f .deps/parse.Tpo .deps/parse.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT statement.lo -MD -MP -MF .deps/statement.Tpo -c -o statement.lo statement.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT statement.lo -MD -MP -MF .deps/statement.Tpo -c statement.c  -fPIC -DPIC -o .libs/statement.o
mv -f .deps/statement.Tpo .deps/statement.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT tuple.lo -MD -MP -MF .deps/tuple.Tpo -c -o tuple.lo tuple.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT tuple.lo -MD -MP -MF .deps/tuple.Tpo -c tuple.c  -fPIC -DPIC -o .libs/tuple.o
mv -f .deps/tuple.Tpo .deps/tuple.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT dlg_specific.lo -MD -MP -MF .deps/dlg_specific.Tpo -c -o dlg_specific.lo dlg_specific.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT dlg_specific.lo -MD -MP -MF .deps/dlg_specific.Tpo -c dlg_specific.c  -fPIC -DPIC -o .libs/dlg_specific.o
mv -f .deps/dlg_specific.Tpo .deps/dlg_specific.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT multibyte.lo -MD -MP -MF .deps/multibyte.Tpo -c -o multibyte.lo multibyte.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT multibyte.lo -MD -MP -MF .deps/multibyte.Tpo -c multibyte.c  -fPIC -DPIC -o .libs/multibyte.o
mv -f .deps/multibyte.Tpo .deps/multibyte.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT odbcapi.lo -MD -MP -MF .deps/odbcapi.Tpo -c -o odbcapi.lo odbcapi.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT odbcapi.lo -MD -MP -MF .deps/odbcapi.Tpo -c odbcapi.c  -fPIC -DPIC -o .libs/odbcapi.o
mv -f .deps/odbcapi.Tpo .deps/odbcapi.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT descriptor.lo -MD -MP -MF .deps/descriptor.Tpo -c -o descriptor.lo descriptor.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT descriptor.lo -MD -MP -MF .deps/descriptor.Tpo -c descriptor.c  -fPIC -DPIC -o .libs/descriptor.o
mv -f .deps/descriptor.Tpo .deps/descriptor.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT odbcapi30.lo -MD -MP -MF .deps/odbcapi30.Tpo -c -o odbcapi30.lo odbcapi30.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT odbcapi30.lo -MD -MP -MF .deps/odbcapi30.Tpo -c odbcapi30.c  -fPIC -DPIC -o .libs/odbcapi30.o
mv -f .deps/odbcapi30.Tpo .deps/odbcapi30.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT pgapi30.lo -MD -MP -MF .deps/pgapi30.Tpo -c -o pgapi30.lo pgapi30.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT pgapi30.lo -MD -MP -MF .deps/pgapi30.Tpo -c pgapi30.c  -fPIC -DPIC -o .libs/pgapi30.o
mv -f .deps/pgapi30.Tpo .deps/pgapi30.Plo
/bin/sh ./libtool  --tag=CC   --mode=compile gcc -DHAVE_CONFIG_H -I.   -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal  -g -O2 -Wall -MT mylog.lo -MD -MP -MF .deps/mylog.Tpo -c -o mylog.lo mylog.c
libtool: compile:  gcc -DHAVE_CONFIG_H -I. -DSQLCOLATTRIBUTE_SQLLEN -I/usr/include -I/usr/include -I/usr/include/pgsql/internal -g -O2 -Wall -MT mylog.lo -MD -MP -MF .deps/mylog.Tpo -c mylog.c  -fPIC -DPIC -o .libs/mylog.o
mv -f .deps/mylog.Tpo .deps/mylog.Plo
/bin/sh ./libtool  --tag=CC   --mode=link gcc  -g -O2 -Wall -module -no-undefined -avoid-version -export-symbols-regex '^SQL' -L/usr/lib64 -L/usr/lib64 -o psqlodbca.la -rpath /usr/lib64 info.lo bind.lo columninfo.lo connection.lo convert.lo drvconn.lo environ.lo execute.lo lobj.lo misc.lo options.lo pgtypes.lo psqlodbc.lo qresult.lo results.lo parse.lo statement.lo tuple.lo dlg_specific.lo multibyte.lo odbcapi.lo descriptor.lo odbcapi30.lo pgapi30.lo mylog.lo  -lpq -lpthread -lodbcinst 
libtool: link: /usr/bin/nm -B  .libs/info.o .libs/bind.o .libs/columninfo.o .libs/connection.o .libs/convert.o .libs/drvconn.o .libs/environ.o .libs/execute.o .libs/lobj.o .libs/misc.o .libs/options.o .libs/pgtypes.o .libs/psqlodbc.o .libs/qresult.o .libs/results.o .libs/parse.o .libs/statement.o .libs/tuple.o .libs/dlg_specific.o .libs/multibyte.o .libs/odbcapi.o .libs/descriptor.o .libs/odbcapi30.o .libs/pgapi30.o .libs/mylog.o   | sed -n -e 's/^.*[	 ]\([ABCDGIRSTW][ABCDGIRSTW]*\)[	 ][	 ]*\([_A-Za-z][_A-Za-z0-9]*\)$/\1 \2 \2/p' | sed '/ __gnu_lto/d' | /usr/bin/sed 's/.* //' | sort | uniq > .libs/psqlodbca.exp
libtool: link: /usr/bin/grep -E -e "^SQL" ".libs/psqlodbca.exp" > ".libs/psqlodbca.expT"
libtool: link: mv -f ".libs/psqlodbca.expT" ".libs/psqlodbca.exp"
libtool: link: echo "{ global:" > .libs/psqlodbca.ver
libtool: link:  cat .libs/psqlodbca.exp | sed -e "s/\(.*\)/\1;/" >> .libs/psqlodbca.ver
libtool: link:  echo "local: *; };" >> .libs/psqlodbca.ver
libtool: link:  gcc -shared  -fPIC -DPIC  .libs/info.o .libs/bind.o .libs/columninfo.o .libs/connection.o .libs/convert.o .libs/drvconn.o .libs/environ.o .libs/execute.o .libs/lobj.o .libs/misc.o .libs/options.o .libs/pgtypes.o .libs/psqlodbc.o .libs/qresult.o .libs/results.o .libs/parse.o .libs/statement.o .libs/tuple.o .libs/dlg_specific.o .libs/multibyte.o .libs/odbcapi.o .libs/descriptor.o .libs/odbcapi30.o .libs/pgapi30.o .libs/mylog.o   -L/usr/lib64 -lpq -lpthread -lodbcinst  -O2   -Wl,-soname -Wl,psqlodbca.so -Wl,-version-script -Wl,.libs/psqlodbca.ver -o .libs/psqlodbca.so
libtool: link: ( cd ".libs" && rm -f "psqlodbca.la" && ln -s "../psqlodbca.la" "psqlodbca.la" )
make[1]: 离开目录“/mnt/psqlodbc-16.00.0000”
[root@localhost psqlodbc-16.00.0000]# make install
make[1]: 进入目录“/mnt/psqlodbc-16.00.0000”
 /usr/bin/mkdir -p '/usr/lib64'
 /bin/sh ./libtool   --mode=install /usr/bin/install -c   psqlodbcw.la psqlodbca.la '/usr/lib64'
libtool: install: /usr/bin/install -c .libs/psqlodbcw.so /usr/lib64/psqlodbcw.so
libtool: install: /usr/bin/install -c .libs/psqlodbcw.lai /usr/lib64/psqlodbcw.la
libtool: install: /usr/bin/install -c .libs/psqlodbca.so /usr/lib64/psqlodbca.so
libtool: install: /usr/bin/install -c .libs/psqlodbca.lai /usr/lib64/psqlodbca.la
libtool: finish: PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/sbin" ldconfig -n /usr/lib64
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib64

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the `LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the `LD_RUN_PATH' environment variable
     during linking
   - use the `-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to `/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
make[1]: 对“install-data-am”无需做任何事。
make[1]: 离开目录“/mnt/psqlodbc-16.00.0000”
[root@localhost psqlodbc-16.00.0000]# 

```

查看odbc配置文件：

```
[root@localhost psqlodbc-16.00.0000]# cat /etc/odbcinst.ini 
# Example driver definitions

# Driver from the postgresql-odbc package
# Setup from the unixODBC package
[PostgreSQL]
Description	= ODBC for PostgreSQL
Driver		= /usr/lib/psqlodbcw.so
Setup		= /usr/lib/libodbcpsqlS.so
Driver64	= /usr/lib64/psqlodbcw.so
Setup64		= /usr/lib64/libodbcpsqlS.so
FileUsage	= 1


# Driver from the mysql-connector-odbc package
# Setup from the unixODBC package
[MySQL]
Description	= ODBC for MySQL
Driver		= /usr/lib/libmyodbc5.so
Setup		= /usr/lib/libodbcmyS.so
Driver64	= /usr/lib64/libmyodbc5.so
Setup64		= /usr/lib64/libodbcmyS.so
FileUsage	= 1


# Driver from the freetds-libs package
# Setup from the unixODBC package
[FreeTDS]
Description     = Free Sybase & MS SQL Driver
Driver          = /usr/lib/libtdsodbc.so
Setup           = /usr/lib/libtdsS.so
Driver64        = /usr/lib64/libtdsodbc.so
Setup64         = /usr/lib64/libtdsS.so
Port            = 1433


# Driver from the mariadb-connector-odbc package
# Setup from the unixODBC package
[MariaDB]
Description     = ODBC for MariaDB
Driver          = /usr/lib/libmaodbc.so
Driver64        = /usr/lib64/libmaodbc.so
FileUsage       = 1
[root@localhost psqlodbc-16.00.0000]# 
```

配置oracle用户的odbc.ini


```
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/hs/admin]$ cat ~/.odbc.ini
[pglink41]
Description = PostgresSQLODBC
Driver = PostgreSQL
Database = lyradb
Servername = 192.168.88.201
UserName = lyra_ops
Password = pVc7EPshyba5
Port = 5432
ReadOnly = 0
```

```
[root@localhost psqlodbc-16.00.0000]# su - oracle
[oracle@orcl:/home/oracle]$ isql -v pglink41
+---------------------------------------+
| Connected!                            |
|                                       |
| sql-statement                         |
| help [tablename]                      |
| quit                                  |
|                                       |
+---------------------------------------+
SQL> quit
[oracle@orcl:/home/oracle]$  


```


二、配置initpglink41.ora文件

```
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/hs/admin]$ pwd
/u01/app/oracle/product/19.3.0/db/hs/admin
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/hs/admin]$ pwd
/u01/app/oracle/product/19.3.0/db/hs/admin
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/hs/admin]$ ls
extproc.ora  initdg4odbc.ora  initpglink41.ora  listener.ora.sample  tnsnames.ora.sample
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/hs/admin]$ cat initpglink41.ora 
HS_FDS_CONNECT_INFO=pglink41
HS_FDS_TRACE_LEVEL=255
HS_FDS_SHAREABLE_NAME=/usr/lib64/psqlodbcw.so 
HS_NLS_NCHAR=UCS2                                     
HS_LANGUAGE=AMERICAN_AMERICA.AL32UTF8                 
set ODBCINI=/home/oracle/.odbc.ini
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/hs/admin]$ 
```

配置监听：

```

[oracle@orcl:/u01/app/oracle/product/19.3.0/db]$ cd network/admin/
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/network/admin]$ cat listener.ora 
# listener.ora Network Configuration File: /u01/app/oracle/product/19.3.0/db/network/admin/listener.ora
# Generated by Oracle configuration tools.


LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = orcl)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )



SID_LIST_LISTENER =
   (SID_LIST =
     (SID_DESC =
     (SID_NAME = orcl)
     (ORACLE_HOME = /u01/app/oracle/product/19.3.0/db)
    )
     (SID_DESC =
     (GLOBAL_DBNAME = pglink41)
     (PROGRAM = dg4odbc)
     (SID_NAME = pglink41)
     (ORACLE_HOME = /u01/app/oracle/product/19.3.0/db)
    )	
  )

  
```

重启监听：


```
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/network/admin]$ lsnrctl status

LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 12-DEC-2024 15:55:01

Copyright (c) 1991, 2019, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=orcl)(PORT=1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 19.0.0.0.0 - Production
Start Date                12-DEC-2024 13:37:56
Uptime                    0 days 2 hr. 17 min. 4 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/19.3.0/db/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/orcl/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=orcl)(PORT=1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
Services Summary...
Service "orcl" has 2 instance(s).
  Instance "orcl", status UNKNOWN, has 1 handler(s) for this service...
  Instance "orcl", status READY, has 1 handler(s) for this service...
Service "orclXDB" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
Service "pglink41" has 1 instance(s).
  Instance "pglink41", status UNKNOWN, has 1 handler(s) for this service...
The command completed successfully



```

配置tnsnames.ora进行测试

```
[oracle@orcl:/u01/app/oracle/product/19.3.0/db/network/admin]$ cat tnsnames.ora 
# tnsnames.ora Network Configuration File: /u01/app/oracle/product/19.3.0/db/network/admin/tnsnames.ora
# Generated by Oracle configuration tools.

LISTENER_ORCL =
  (ADDRESS = (PROTOCOL = TCP)(HOST = orcl)(PORT = 1521))


ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = orcl)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )



pglink41 =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.99.201)(PORT = 1521))
    (CONNECT_DATA = (SID = pglink41))
    (HS = OK)
  )




[oracle@orcl:/home/oracle]$ tnsping pglink41

TNS Ping Utility for Linux: Version 19.0.0.0.0 - Production on 12-DEC-2024 17:49:42

Copyright (c) 1997, 2019, Oracle.  All rights reserved.

Used parameter files:
/u01/app/oracle/product/19.3.0/db/network/admin/sqlnet.ora


Used TNSNAMES adapter to resolve the alias
Attempting to contact (DESCRIPTION = (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.99.201)(PORT = 1521)) (CONNECT_DATA = (SID = pglink41)) (HS = OK))
OK (0 msec)
[oracle@orcl:/home/oracle]$ 
```

创建数据库dblink：

```
create public database link dblink_pg41 connect to "lyra_ops" identified by "pVc7EPshyba5" using '(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=192.168.99.201)(PORT=1521))(CONNECT_DATA=(SID = pglink41))(HS = OK))'；

```

问题1:

```
[psqlodbc-REL-16_00_0004][Postgres 15 5][Debian][unixODBC] Need to add CPPFLAGS="-DSQLCOLATTRIBUTE_SQLLEN" to configure to get it to compile
```

引用解决方案：> 