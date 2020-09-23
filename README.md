<div align="center">

## Rudimentary UDP with Winsocks


</div>

### Description

The article demonstrates using Winsocks for rudimentary communication with UDP
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Ixac](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/ixac.md)
**Level**          |Beginner
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |Microsoft Visual C\+\+
**Category**       |[System Services/ Functions](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/system-services-functions__3-23.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/ixac-rudimentary-udp-with-winsocks__3-7111/archive/master.zip)





### Source Code

<table border="0" cellpadding="5" cellspacing="0" style="border-collapse: collapse" bordercolor="#111111" width="600" id="AutoNumber1">
 <tr>
  <td width="100%"><b><font face="Verdana" size="4" color="#000080">Rudimentary UDP with
  Winsocks</font></b><p><font face="Verdana" size="2">Two simple console based
  programs demonstrate UDP with Winsocks. The client simply sends packets of
  data typed in by the user and the server displays it. The code is more of a
  reference for starting applications that require Winsocks. </font></p>
  <table border="1" cellpadding="5" cellspacing="0" style="border-collapse: collapse" bordercolor="#CCCCCC" width="100%" id="AutoNumber2" bgcolor="#CCCCCC">
   <tr>
    <td width="100%" bgcolor="#999999" bordercolor="#999999"><b>
    <font face="Verdana" color="#FFFFFF">Server Code</font></b><ul>
     <li><font face="Verdana" size="2" color="#FFFFFF">Create a new Win32
     Console application in VC++ 6.0.</font></li>
     <li><font face="Verdana" size="2" color="#FFFFFF">Add a new CPP file
     to this project.</font></li>
     <li><font face="Verdana" size="2" color="#FFFFFF">Copy and paste the
     Server code in this new file and compile the project.</font></li>
    </ul>
    </td>
   </tr>
   <tr>
    <td width="100%"><font size="2" face="Courier New">
    <font color="#0000FF">#include</font> &lt;iostream.h&gt;<br>
    <font color="#0000FF">#include</font> &lt;Winsock2.h&gt;<br>
    <br>
    <font color="#0000FF">void</font> ReportError (<font color="#0000FF">const
    char* const</font> szErr)<br>
    {<br>
&nbsp; cout &lt;&lt; szErr &lt;&lt; endl;<br>
    }<br>
    <br>
    <font color="#0000FF">int</font> main (<font color="#0000FF">void</font>)<br>
    {<br>
&nbsp; <font color="#008080">// Initialize Windows Sockets</font><br>
&nbsp; WORD wVersionRequired = MAKEWORD (2, 2);<br>
&nbsp; WSADATA wd;<br>
&nbsp; <font color="#0000FF">int</font> iReturn = ::WSAStartup (wVersionRequired,
    &amp;wd);<br>
&nbsp; <font color="#0000FF">if</font> (iReturn != 0)<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">return</font> ReportError (&quot;WSAStartup
    failed.&quot;), 0;<br>
    <br>
&nbsp; <font color="#008080">// Create the socket</font><br>
&nbsp; SOCKET sock = ::WSASocket (AF_INET, SOCK_DGRAM, 0, 0, 0, 0);<br>
&nbsp; <font color="#0000FF">if</font> (sock == INVALID_SOCKET)<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">return</font> ReportError (&quot;WSASocket
    failed.&quot;), ::WSACleanup (), 0;<br>
    <br>
&nbsp; <font color="#008080">// Get the name of the local computer</font><br>
&nbsp; <font color="#0000FF">char</font> szHostName [MAX_PATH];<br>
&nbsp; ::gethostname (szHostName, <font color="#0000FF">sizeof</font> szHostName);<br>
    <br>
&nbsp; <font color="#008080">// Fill in the sockaddr_in structure for binding<br>
    </font>&nbsp; sockaddr_in sa;<br>
&nbsp; sa.sin_family = AF_INET;<br>
&nbsp; ::WSAHtons (sock, 8080, &amp;sa.sin_port);<br>
    <br>
&nbsp; <font color="#008080">// Get the IP address of the local computer name<br>
    </font>&nbsp; HOSTENT* phe = ::gethostbyname (szHostName);<br>
&nbsp; memcpy (&amp;sa.sin_addr, phe-&gt;h_addr_list [0], phe-&gt;h_length);<br>
    <br>
&nbsp; <font color="#008080">// Pad with zeros</font><br>
&nbsp; ZeroMemory (sa.sin_zero, <font color="#0000FF">sizeof</font> sa.sin_zero);<br>
    <br>
&nbsp; <font color="#008080">// Bind to a port</font><br>
&nbsp; iReturn = ::bind (sock, <font color="#0000FF">reinterpret_cast</font> &lt;sockaddr*&gt;
    (&amp;sa), <br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">sizeof</font> sockaddr_in);<br>
&nbsp; <font color="#0000FF">if</font> (iReturn == SOCKET_ERROR)<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">return</font> ReportError (&quot;bind
    failed.&quot;), ::closesocket (sock), <br>
&nbsp;&nbsp;&nbsp; ::WSACleanup (), 0;<br>
    <br>
&nbsp; <font color="#008080">// Prepare receive buffer(s)</font><br>
&nbsp; WSABUF wbRecv [1];<br>
&nbsp; <font color="#0000FF">char</font> szRecvBuf [1][256];<br>
&nbsp; wbRecv [0].len = 256;<br>
&nbsp; wbRecv [0].buf = szRecvBuf [0];<br>
&nbsp;<br>
&nbsp; <font color="#008080">// Receive the first line of data</font><br>
&nbsp; DWORD dwBytesReceived = 0, dwFlags = 0;<br>
&nbsp; INT nFromLen = <font color="#0000FF">sizeof</font> sockaddr_in;<br>
&nbsp; if ((iReturn = ::WSARecvFrom (sock, wbRecv, 1, <br>
&nbsp;&nbsp;&nbsp; &amp;dwBytesReceived, &amp;dwFlags, <br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">reinterpret_cast</font> &lt;sockaddr*&gt; (&amp;sa),
    &amp;nFromLen, <br>
&nbsp;&nbsp;&nbsp; NULL, NULL)) == 0)<br>
&nbsp; {<br>
&nbsp;&nbsp;&nbsp; <font color="#008080">// Output Received data till a 'Close'
    is received.</font><br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">do</font><br>
&nbsp;&nbsp;&nbsp; {<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; cout &lt;&lt; &quot;Data: &quot; &lt;&lt; wbRecv [0].buf &lt;&lt; &quot;, Read: &quot;
    <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;&lt; dwBytesReceived &lt;&lt; &quot; bytes&quot; &lt;&lt;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &quot;, From IP: &quot; &lt;&lt; ::inet_ntoa (sa.sin_addr)
    &lt;&lt; endl;<br>
    <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <font color="#008080">// Reset in - out
    parameters</font><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; nFromLen = <font color="#0000FF">sizeof</font>
    sockaddr_in;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; dwFlags = 0;<br>
&nbsp;&nbsp;&nbsp; }<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">while</font> (lstrcmpi (wbRecv [0].buf,
    &quot;Close&quot;) != 0<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &amp;&amp; ::WSARecvFrom (sock, wbRecv, 1, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &amp;dwBytesReceived, &amp;dwFlags, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <font color="#0000FF">reinterpret_cast</font> &lt;sockaddr*&gt;
    (&amp;sa), &amp;nFromLen, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; NULL, NULL) == 0);<br>
&nbsp; }<br>
&nbsp; <font color="#0000FF">else</font><br>
&nbsp; {<br>
&nbsp;&nbsp;&nbsp; <font color="#008080">// Report Recv errors</font><br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">char</font> szError [256];<br>
&nbsp;&nbsp;&nbsp; wsprintf (szError, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &quot;WSARecvFrom failed and returned: %d. &quot;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &quot;WSAGetLastError: %d&quot;, iReturn, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WSAGetLastError ());<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ReportError (szError);<br>
&nbsp; }<br>
    <br>
&nbsp; <font color="#008080">// Close the socket</font><br>
&nbsp; ::closesocket (sock);<br>
&nbsp;<br>
&nbsp; <font color="#008080">// Uninitialize Windows Sockets</font><br>
&nbsp; ::WSACleanup ();<br>
    <br>
&nbsp; <font color="#0000FF">return</font> 0;<br>
    }</font></td>
   </tr>
  </table>
  <table border="0" cellpadding="0" cellspacing="0" style="border-collapse: collapse" bordercolor="#111111" width="100%" id="AutoNumber4">
   <tr>
    <td width="100%">&nbsp;</td>
   </tr>
  </table>
  <table border="1" cellpadding="5" cellspacing="0" style="border-collapse: collapse" bordercolor="#CCCCCC" width="100%" id="AutoNumber3" bgcolor="#CCCCCC">
   <tr>
    <td width="100%" bgcolor="#999999" bordercolor="#999999"><b>
    <font face="Verdana" color="#FFFFFF">Client Code</font></b><ul>
     <li><font face="Verdana" size="2" color="#FFFFFF">Create another new
     Win32 Console application in VC++ 6.0.</font></li>
     <li><font face="Verdana" size="2" color="#FFFFFF">Add a new CPP file
     to this project.</font></li>
     <li><font face="Verdana" size="2" color="#FFFFFF">Copy and paste the
     Client code in this new file and compile the project.</font></li>
    </ul>
    </td>
   </tr>
   <tr>
    <td width="100%"><font face="Courier New" size="2">
    <font color="#0000FF">#include</font> &lt;iostream.h&gt;<br>
    <font color="#0000FF">#include</font> &lt;Winsock2.h&gt;<br>
    <font color="#0000FF">#include</font> &lt;stdio.h&gt;<br>
    <br>
    <font color="#0000FF">void</font> ReportError (<font color="#0000FF">const</font>
    <font color="#0000FF">char*</font> <font color="#0000FF">const</font>
    szErr)<br>
    {<br>
&nbsp; cout &lt;&lt; szErr &lt;&lt; endl;<br>
    }<br>
    <br>
    <font color="#0000FF">int</font> main (<font color="#0000FF">void</font>)<br>
    {<br>
&nbsp; <font color="#008080">// Initialize Windows Sockets</font><br>
&nbsp; WORD wVersionRequired = MAKEWORD (2, 2);<br>
&nbsp; WSADATA wd;<br>
&nbsp; <font color="#0000FF">int</font> iReturn = ::WSAStartup (wVersionRequired,
    &amp;wd);<br>
&nbsp; <font color="#0000FF">if</font> (iReturn != 0)<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">return</font> ReportError (&quot;WSAStartup
    failed.&quot;), 0;<br>
    <br>
&nbsp; <font color="#008080">// Create the socket</font><br>
&nbsp; SOCKET sock = ::WSASocket (AF_INET, SOCK_DGRAM, 0, 0, 0, 0);<br>
&nbsp; <font color="#0000FF">if</font> (sock == INVALID_SOCKET)<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">return</font> ReportError (&quot;WSASocket
    failed.&quot;), ::WSACleanup (), 0;<br>
    <br>
&nbsp; <font color="#008080">// Fill in the sockaddr_in structure for sending</font><br>
&nbsp; sockaddr_in sa;<br>
&nbsp; sa.sin_family = AF_INET;<br>
&nbsp; ::WSAHtons (sock, 8080, &amp;sa.sin_port);<br>
    <br>
&nbsp; <font color="#008080">// Fill in the IP address of the machine where the
    <br>
&nbsp; // server resides</font><br>
&nbsp; sa.sin_addr.S_un.S_addr = ::inet_addr (&quot;xxx.xxx.xxx.xxx&quot;);<br>
    <font color="#008080">&nbsp; // Substitute &quot;xxx.xxx.xxx.xxx&quot; with a
    valid IP</font><br>
    <br>
&nbsp; <font color="#008080">// Pad with zeros</font><br>
&nbsp; ZeroMemory (sa.sin_zero, <font color="#0000FF">sizeof</font> sa.sin_zero);<br>
    <br>
&nbsp; <font color="#008080">// Prepare receive buffer(s)</font><br>
&nbsp; WSABUF wbSend [1];<br>
&nbsp; <font color="#0000FF">char</font> szSendBuf [1][512];<br>
&nbsp; wbSend [0].len = 512;<br>
&nbsp; wbSend [0].buf = szSendBuf [0];<br>
    <br>
&nbsp; <font color="#008080">// Receive the first line of data</font><br>
&nbsp; DWORD dwBytesSent = 0;<br>
&nbsp; INT nToLen = <font color="#0000FF">sizeof</font> sockaddr_in;<br>
    <br>
&nbsp; cout &lt;&lt; &quot;&gt;&quot; &lt;&lt; flush;<br>
&nbsp; <font color="#0000FF">char</font> szGet [402];<br>
&nbsp; gets (szGet);<br>
    <br>
&nbsp; DWORD dwTickCount = GetTickCount ();<br>
&nbsp; wsprintf (wbSend [0].buf, &quot;%lu %s&quot;, dwTickCount, szGet);<br>
    <br>
&nbsp; <font color="#0000FF">if</font> ((iReturn = ::WSASendTo (sock, wbSend, 1,
    <br>
&nbsp;&nbsp;&nbsp; &amp;dwBytesSent, NULL, <br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">reinterpret_cast</font> &lt;sockaddr*&gt; (&amp;sa),
    <br>
&nbsp;&nbsp;&nbsp; nToLen, NULL, NULL)) == 0)<br>
&nbsp; {<br>
&nbsp;&nbsp;&nbsp; <font color="#008080">// Output Sent data till an 'Exit' is
    typed</font><br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">do</font><br>
&nbsp;&nbsp;&nbsp; {<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; cout &lt;&lt; &quot;Data: &quot; &lt;&lt; wbSend [0].buf &lt;&lt; &quot;, Sent: &quot;
    <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;&lt; dwBytesSent &lt;&lt; endl;<br>
    <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; cout &lt;&lt; &quot;&gt;&quot; &lt;&lt; flush;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; gets (szGet);<br>
    <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; dwTickCount = GetTickCount ();<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; wsprintf (wbSend [0].buf, &quot;%lu %s&quot;, dwTickCount,
    szGet);<br>
    <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <font color="#008080">// Reset in - out
    parameters</font><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; nToLen = sizeof sockaddr_in;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; dwBytesSent = 0;<br>
&nbsp;&nbsp;&nbsp; }<br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">while</font> (lstrcmpi (szGet, &quot;Exit&quot;)
    != 0<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &amp;&amp; ::WSASendTo (sock, wbSend, 1, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &amp;dwBytesSent, NULL, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <font color="#0000FF">reinterpret_cast</font> &lt;sockaddr*&gt;
    (&amp;sa), <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; nToLen, NULL, NULL) == 0);<br>
&nbsp; }<br>
&nbsp; <font color="#0000FF">else</font><br>
&nbsp; {<br>
&nbsp;&nbsp;&nbsp; <font color="#008080">// Report Recv errors</font><br>
&nbsp;&nbsp;&nbsp; <font color="#0000FF">char</font> szError [256];<br>
&nbsp;&nbsp;&nbsp; wsprintf (szError, <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &quot;WSASendTo failed and returned: %d. &quot;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &quot;WSAGetLastError: %d&quot;, iReturn, <br>
&nbsp;&nbsp;&nbsp; WSAGetLastError ());<br>
&nbsp;&nbsp;&nbsp; ReportError (szError);<br>
&nbsp; }<br>
    <br>
&nbsp; <font color="#008080">// Close the socket</font><br>
&nbsp; ::closesocket (sock);<br>
    <br>
&nbsp; <font color="#008080">// Uninitialize Windows Sockets</font><br>
&nbsp; ::WSACleanup ();<br>
    <br>
&nbsp; <font color="#0000FF">return</font> 0;<br>
    }</font></td>
   </tr>
  </table>
  <p><font face="Verdana" size="2">Once the client and the server program is
  compiled then run both at the same time. Make sure that the IP used in the
  client is correct. Also make sure that the port number used in the server as
  well as the client is the same.</font></p>
  </td>
 </tr>
</table>

