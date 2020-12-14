


---------------------------------------------- NOTICE ----------------------------------------------
---------------------------------------------- NOTICE ----------------------------------------------
---------------------------------------------- NOTICE ----------------------------------------------
  Starting Sunday 5/25/2014, shortpasta-icmp development is limited to bug fixes!
  shortpasta-icmp builds 1000-1006 are available here:
    https://sourceforge.net/projects/shortpasta-icmp/files/

  shortpasta-icmp2, the 2nd generation implementation of shorpasta-icmp, has now moved to its own project: icmp4j!
  https://sourceforge.net/projects/shortpasta-icmp/files/
  icmp4j is also hosted on sf.net - thanks sf.net!
  
  Check-out icmp4j directly:
    http://www.icmp4j.org, or
    https://sourceforge.net/projects/icmp4j/
---------------------------------------------- /NOTICE ---------------------------------------------
---------------------------------------------- /NOTICE ---------------------------------------------
---------------------------------------------- /NOTICE ---------------------------------------------













/**
 * ShortPasta Foundation
 * http://www.shortpasta.org
 * Copyright 2009 and beyond, Sal Ingrilli at the ShortPasta Software Foundation
 * <p/>
 * This is free software; you can redistribute it and/or modify it
 * under the terms of the GNU Lesser General Public License version 3
 * as published by the Free Software Foundation as long as:
 * 1. You credit the original author somewhere within your product or website
 * 2. The credit is easily reachable and not burried deep
 * 3. Your end-user can easily see it
 * 4. You register your name (optional) and company/group/org name (required)
 * at http://www.shortpasta.org
 * 5. You do all of the above within 4 weeks of integrating this software
 * 6. You contribute feedback, fixes, and requests for features
 * <p/>
 * If/when you derive a commercial gain from using this software
 * please donate at http://www.shortpasta.org
 * <p/>
 * If prefer or require, contact the author specified above to:
 * 1. Release you from the above requirements
 * 2. Acquire a commercial license
 * 3. Purchase a support contract
 * 4. Request a different license
 * 5. Anything else
 * <p/>
 * This software is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE, similarly
 * to how this is described in the GNU Lesser General Public License.
 * <p/>
 * User: Sal Ingrilli
 * Date: May 23, 2014
 * Time: 1:00:53 PM
 */
 
###
### Project details
###
Compilers:
  Java code built with Sun Java 1.7.0_55-b13, but compiled for 1.6
  Apache Ant 1.9.2

Binaries:
  jna-3.5.1.jar (677 KB)
  platform-3.5.1.jar (931 KB)
  shortpasta-icmp2.jar (23 KB)
  
Tested platforms:
  Windows 8.1 64-bit 
  Windows Server 2008r2 64-bit 
  Windows 7 Pro 64-bit (ver 6.1.7601)
  Windows XP Pro 32-bit
  Debian 6
  
Running from the command line:
  1. Expand shortpasta-icmp2-1013.zip to c:\temp
  2. cmd
  3. cd C:\temp\shortpasta-icmp2\lib
  4. java -cp * org.shortpasta.icmp2.tool.Ping www.google.com
     -or, if the above does not work:
  4. java -cp jna-3.5.1.jar;platform-3.5.1.jar;shortpasta-icmp2.jar org.shortpasta.icmp2.tool.Ping www.google.com

Sample code:
  import org.shortpasta.icmp2.IcmpPingUtil;
  import org.shortpasta.icmp2.IcmpPingRequest;
  import org.shortpasta.icmp2.IcmpPingResponse;
  
  // request - use IcmpPingUtil.createIcmpPingRequest () to create a request with defaults
  final IcmpPingRequest request = IcmpPingUtil.createIcmpPingRequest ();
  request.setHost ("www.google.com");
  
  // delegate
  final IcmpPingResponse response = IcmpPingUtil.executePingRequest (request);
  
  // log
  final String formattedResponse = IcmpPingUtil.formatResponse (response);
  System.out.println (formattedResponse);
  
###
### Date: 2014-05-25 14:43:00
### Build: 1015
###
  Starting Sunday 5/25/2014, shortpasta-icmp development is limited to bug fixes!
  shortpasta-icmp builds 1000-1006 are available here:
    https://sourceforge.net/projects/shortpasta-icmp/files/

  shortpasta-icmp2, the 2nd generation implementation of shorpasta-icmp, has now moved to its own project: icmp4j!
  https://sourceforge.net/projects/shortpasta-icmp/files/
  icmp4j is also hosted on sf.net - thanks sf.net!
  
  Check-out icmp4j directly:
    http://www.icmp4j.org, or
    https://sourceforge.net/projects/icmp4j/

###
### Date: 2014-04-25 10:34:00
### Build: 1014
###
-- [FEATURE]
  Add explicit timeout support
  Te Windows implementation relies on the native OS call, and it seems to report the timeout a few msecs before
  the actual timeout. For example, pinging a non-existing domain with a timeout of 5000, came back yielded 
  a timeout after 4935, 4996, 4996, and 4997 msecs instead of 5000.
  The Linux/Solaris/Other implementations use an explicit timer on the Java side.

-- [FEATURE]
  Add several preconditions and postcondition checks in IcmpPingUtil.executePingRequest (request);

-- [FEATURE]
  Add auto initialization: when you invoke IcmpPingUtil.executePingRequest (), it initializes the subsystem.
  However, if the initialization fails for any reason, such as winsock2 is not available, you will receive
  an exception, so you may still want to call the following at app startup so that you are guaranteed
  that the icmp functions are available:
    IcmpPingUtil.initialize ();
  Likewise, you can call the following at app tear-down, but it is not required:
    IcmpPingUtil.destroy ();

###
### Date: 2014-04-23 16:20:00
### Build: 1014
###
-- [FEATURE]
  Add Linux/Solaris support
  See the LinuxNativeBridge class, which executes the following command:
    ping -c 1 -s 32 -w 5 www.google.com
  This command sends 1 packet of size 32 with a timeout of 5 seconds
  
-- [FEATURE]
  Add unspecified OS support
  If the OS is not Windows/Linux/Solaris, shortpasta-icmp2 uses Java default process
  See the JavaNativeBridge class, which delegates to InetAddress.isReachable ().

-- Credits
  Kevin Shih
    Help with testing and integration

###
### TODOs
###
Look into IcmpSendEcho2Ex so that we can specify the source ip address, which indirectly lets you specify the interface:
http://msdn.microsoft.com/en-us/library/windows/desktop/bb309069(v=vs.85).aspx
Sample c# code by Jason Harrop:
https://github.com/plutext/PlotPing/commit/5e468bb388c42bd1cd565a913808462132db5039

###
### Date: 2014-04-23 13:00:00
### Build: 1013
###
-- [WARNING]
  In build 1013, shortpasta-icmp is now deprecated and replaced by shortpasta-icmp2.

  About shortpasta-icmp:
  is a very small java wrapper around a small native implementation for win32 and uses JNI.
  Jar + Win32 DLL.
  Stand-alone and small footprint: shortpasta-icmp.jar (47k) + shortpasta-icmp-win32.dll (68k release) + shortpasta-icmp-win64.dll
  Stand-alone project: build with Java 1.6 and Ant 1.7, native code built with VCPP6
  Uses real ICMP sockets via native calls which offers very accurate delay calcs
  Why not use jna or other wrappers? They usually come with larger payloads. Still very good options for apps where size is not a concern
  Unit tested including simulations of millions of packets
  Extremely accurate timing via QueryPerformanceFrequency () and QueryPerformanceCounter () with fallback to GetTickCount ()
  Log4J support
  Asynchronous ping support
  Standalone sping.exe with /xml option to output results in xml

  About shortpasta-icmp2:
  Very small java wrapper around the native win32 methods and uses JNA to access them.
  So although shortpasta-icmp2 is only 25 KB, it requires 1,608 KB worth of jna jars to execute.
  If this is not acceptable to you, use build 1006.

-- [FEATURE]
  Add blocking call:
    IcmpPingResponse org.shortpasta.icmp2.IcmpUtil.executePingRequest (IcmpPingRequest).

  This implementation uses:
  1. JNA to invoke native methods
  2. winsock 2.0
  3. IcmpSendEcho instead of the raw socket () / bind () / send () family of methods

-- Credits
  Kevin Shih
    Help with testing and integration