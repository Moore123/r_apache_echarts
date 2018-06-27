Compile script for R_apache_echarts 

untar http-2.4.xx.  package

in http-2.4.xx directory under srclib 
uncompress apr-1.6.x.tar.bz2
uncompress apr-util-1.6.x.tar.bz2 

generate symbol link 
    ln -s apr apr-util from version above two

download sourcecode package of pcre (NOT pcre2) from https://ftp.pcre.org/pub/pcre/ 
and run comp_pcre  make & make install


#configure options for httpd server
LoadModule R_module  modules/mod_R.so
ROutputErrors
REvalOnStartup "options(warn=-1); library(TTR); library(rjson); options(warn=0)"

<Location /RApacheInfo>
    SetHandler r-info
</Location>

<Directory /var/www/realtime>
    SetHandler r-script
    RHandler brew::brew
</Directory>

<Directory /var/www/R-files>
    SetHandler r-script
    RHandler sys.source
</Directory>

<Location /var/www/runningtest>
    SetHandler r-handler
    RFileHandler /var/www/R-files/runStats.R
</Location>
