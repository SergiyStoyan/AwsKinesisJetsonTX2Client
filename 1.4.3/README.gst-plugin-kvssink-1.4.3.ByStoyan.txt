HELP: GST PLUGIN kvssink ON JETSON TX2

*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
Building amazon-kinesis-video-streams-producer-sdk-1.4.3:
======================================================================================

>./install-script

This script as is has the following issues that require changes:
======================================================================================

'__NR_eventfd' undeclared


remedy: 

in install-script to command ./config add no-afalgeng flag: 

>./config  no-afalgeng --prefix=$DOWNLOADS/local/ --openssldir=$DOWNLOADS/local/


---------------------------------------------------------------------------------------
checking build system type... ./config.guess: unable to guess system type


remedy:

in install-script after command 
    

>cd $DOWNLOADS/m4-1.4.10	

add

>wget -O config.guess 'http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD'
>wget -O config.sub 'http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub;hb=HEAD'

in order to overwrite the obsolete scripts


---------------------------------------------------------------------------------------

curl: (77) error setting certificate verify locations:
  CAfile: /etc/ssl/cert.pem

remedy:

sudo mkdir -p /etc/ssl
sudo cp /etc/ssl/certs/ca-certificates.crt /etc/ssl/cert.pem


*******************************************************************************************************
*******************************************************************************************************
*******************************************************************************************************
How to run gst pipline with kvssink:
=======================================================================================================

Set path to gst-launch-1.0:
>export PATH=/home/ubuntu/amazon-kinesis-video-streams-producer-sdk-cpp-1.4.3/kinesis-video-native-build/downloads/local/bin:$PATH

>export GST_PLUGIN_PATH=/home/ubuntu/amazon-kinesis-video-streams-producer-sdk-cpp-1.4.3/kinesis-video-native-build/downloads/local/lib:$GST_PLUGIN_PATH

>export LD_LIBRARY_PATH=/home/ubuntu/amazon-kinesis-video-streams-producer-sdk-cpp-1.4.3/kinesis-video-native-build/downloads/local/lib:$LD_LIBRARY_PATH:/usr/lib/aarch64-linux-gnu/tegra/:/usr/lib/aarch64-linux-gnu/gstreamer-1.0

>export GST_PLUGIN_PATH=/usr/lib/aarch64-linux-gnu/tegra/:/home/ubuntu/amazon-kinesis-video-streams-producer-sdk-cpp-1.4.3/kinesis-video-native-build/downloads/local/lib/gstreamer-1.0:/usr/lib/aarch64-linux-gnu/gstreamer-1.0:$GST_PLUGIN_PATH

Set access-key and secret-key in the command and run it:
>gst-launch-1.0 nvcamerasrc ! 'video/x-raw(memory:NVMM), width=(int)1920, height=(int)1080, format=(string)I420, framerate=(fraction)24/1' ! omxh264enc ! kvssink access-key= secret-key= stream-name=test11 storage-size=512 

Optionally detach from terminal:
Ctrl+z
>bg


Logging:
======================================================================================

To have managed logging, make sure that kvs_log_configuration file is located in the active directory.
File content for production configuration:

log4cplus.rootLogger=WARN, KvsConsoleAppender
#KvsConsoleAppender:
log4cplus.appender.KvsConsoleAppender=log4cplus::ConsoleAppender
log4cplus.appender.KvsConsoleAppender.layout=log4cplus::PatternLayout
log4cplus.appender.KvsConsoleAppender.layout.ConversionPattern=[%-5p][%d] %m%n



Running issues (if no paths were exported before):
======================================================================================
error while loading shared libraries: liblog4cplus-1.2.so.5: cannot open shared object file: No such file or directory
 
remedy:

>export LD_LIBRARY_PATH=/home/ubuntu/amazon-kinesis-video-streams-producer-sdk-cpp-1.4.3/kinesis-video-native-build/downloads/local/lib:$LD_LIBRARY_PATH:/usr/lib/aarch64-linux-gnu/tegra/:/usr/lib/aarch64-linux-gnu/gstreamer-1.0


--------------------------------------------------------------------------------------
gst_element_factory_make ("nvcamerasrc", NULL); returns NULL 

remedy:

>export GST_PLUGIN_PATH=/usr/lib/aarch64-linux-gnu/tegra/:/home/ubuntu/amazon-kinesis-video-streams-producer-sdk-cpp-1.4.3/kinesis-video-native-build/downloads/local/lib/gstreamer-1.0:/usr/lib/aarch64-linux-gnu/gstreamer-1.0:$GST_PLUGIN_PATH



