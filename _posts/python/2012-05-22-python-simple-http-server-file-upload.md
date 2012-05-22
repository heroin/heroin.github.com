---
layout: post
title: Python支持上传的SimpleHTTPServer
category: python
tags: [python, server, http]
---

在某个目录下运行, 访问[http://127.0.0.1:8000](http://127.0.0.1:8000)

即可对该目录浏览, 或者上传文件.

<pre class="prettyprint linenums">
#!/usr/bin/env python
# -*- coding: UTF-8 -*-

&quot;&quot;&quot;Simple HTTP Server With Upload.

This module builds on BaseHTTPServer by implementing the standard GET
and HEAD requests in a fairly straightforward manner.

&quot;&quot;&quot;


__version__ = &quot;0.1&quot;

import os
import posixpath
import BaseHTTPServer
import urllib
import cgi
import shutil
import mimetypes
import re
try:
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO


class SimpleHTTPRequestHandler(BaseHTTPServer.BaseHTTPRequestHandler):

    &quot;&quot;&quot;Simple HTTP request handler with GET/HEAD/POST commands.

    This serves files from the current directory and any of its
    subdirectories.  The MIME type for files is determined by
    calling the .guess_type() method. And can reveive file uploaded
    by client.

    The GET/HEAD/POST requests are identical except that the HEAD
    request omits the actual contents of the file.

    &quot;&quot;&quot;

    server_version = &quot;SimpleHTTPWithUpload/&quot; + __version__

    def do_GET(self):
        &quot;&quot;&quot;Serve a GET request.&quot;&quot;&quot;
        f = self.send_head()
        if f:
            self.copyfile(f, self.wfile)
            f.close()

    def do_HEAD(self):
        &quot;&quot;&quot;Serve a HEAD request.&quot;&quot;&quot;
        f = self.send_head()
        if f:
            f.close()

    def do_POST(self):
        &quot;&quot;&quot;Serve a POST request.&quot;&quot;&quot;
        r, info = self.deal_post_data()
        print r, info, &quot;by: &quot;, self.client_address
        f = StringIO()
        f.write(&apos;&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD HTML 3.2 Final//EN&quot;&gt;&apos;)
        f.write(&quot;&lt;html&gt;\n&lt;title&gt;Upload Result Page&lt;/title&gt;\n&quot;)
        f.write(&quot;&lt;body&gt;\n&lt;h2&gt;Upload Result Page&lt;/h2&gt;\n&quot;)
        f.write(&quot;&lt;hr&gt;\n&quot;)
        if r:
            f.write(&quot;&lt;strong&gt;Success:&lt;/strong&gt;&quot;)
        else:
            f.write(&quot;&lt;strong&gt;Failed:&lt;/strong&gt;&quot;)
        f.write(info)
        f.write(&quot;&lt;br&gt;&lt;a href=\&quot;%s\&quot;&gt;back&lt;/a&gt;&quot; % self.headers[&apos;referer&apos;])
        f.write(&quot;&lt;hr&gt;&lt;/body&gt;\n&lt;/html&gt;\n&quot;)
        length = f.tell()
        f.seek(0)
        self.send_response(200)
        self.send_header(&quot;Content-type&quot;, &quot;text/html&quot;)
        self.send_header(&quot;Content-Length&quot;, str(length))
        self.end_headers()
        if f:
            self.copyfile(f, self.wfile)
            f.close()
        
    def deal_post_data(self):
        boundary = self.headers.plisttext.split(&quot;=&quot;)[1]
        remainbytes = int(self.headers[&apos;content-length&apos;])
        line = self.rfile.readline()
        remainbytes -= len(line)
        if not boundary in line:
            return (False, &quot;Content NOT begin with boundary&quot;)
        line = self.rfile.readline()
        remainbytes -= len(line)
        fn = re.findall(r&apos;Content-Disposition.*name=&quot;file&quot;; filename=&quot;(.*)&quot;&apos;, line)
        if not fn:
            return (False, &quot;Can&apos;t find out file name...&quot;)
        path = self.translate_path(self.path)
        fn = os.path.join(path, fn[0])
        while os.path.exists(fn):
            fn += &quot;_&quot;
        line = self.rfile.readline()
        remainbytes -= len(line)
        line = self.rfile.readline()
        remainbytes -= len(line)
        try:
            out = open(fn, &apos;wb&apos;)
        except IOError:
            return (False, &quot;Can&apos;t create file to write, do you have permission to write?&quot;)
                
        preline = self.rfile.readline()
        remainbytes -= len(preline)
        while remainbytes &gt; 0:
            line = self.rfile.readline()
            remainbytes -= len(line)
            if boundary in line:
                preline = preline[0:-1]
                if preline.endswith(&apos;\r&apos;):
                    preline = preline[0:-1]
                out.write(preline)
                out.close()
                return (True, &quot;File &apos;%s&apos; upload success!&quot; % fn)
            else:
                out.write(preline)
                preline = line
        return (False, &quot;Unexpect Ends of data.&quot;)

    def send_head(self):
        &quot;&quot;&quot;Common code for GET and HEAD commands.

        This sends the response code and MIME headers.

        Return value is either a file object (which has to be copied
        to the outputfile by the caller unless the command was HEAD,
        and must be closed by the caller under all circumstances), or
        None, in which case the caller has nothing further to do.

        &quot;&quot;&quot;
        path = self.translate_path(self.path)
        f = None
        if os.path.isdir(path):
            if not self.path.endswith(&apos;/&apos;):
                # redirect browser - doing basically what apache does
                self.send_response(301)
                self.send_header(&quot;Location&quot;, self.path + &quot;/&quot;)
                self.end_headers()
                return None
            for index in &quot;index.html&quot;, &quot;index.htm&quot;:
                index = os.path.join(path, index)
                if os.path.exists(index):
                    path = index
                    break
            else:
                return self.list_directory(path)
        ctype = self.guess_type(path)
        try:
            # Always read in binary mode. Opening files in text mode may cause
            # newline translations, making the actual size of the content
            # transmitted *less* than the content-length!
            f = open(path, &apos;rb&apos;)
        except IOError:
            self.send_error(404, &quot;File not found&quot;)
            return None
        self.send_response(200)
        self.send_header(&quot;Content-type&quot;, ctype)
        fs = os.fstat(f.fileno())
        self.send_header(&quot;Content-Length&quot;, str(fs[6]))
        self.send_header(&quot;Last-Modified&quot;, self.date_time_string(fs.st_mtime))
        self.end_headers()
        return f

    def list_directory(self, path):
        &quot;&quot;&quot;Helper to produce a directory listing (absent index.html).

        Return value is either a file object, or None (indicating an
        error).  In either case, the headers are sent, making the
        interface the same as for send_head().

        &quot;&quot;&quot;
        try:
            list = os.listdir(path)
        except os.error:
            self.send_error(404, &quot;No permission to list directory&quot;)
            return None
        list.sort(key=lambda a: a.lower())
        f = StringIO()
        displaypath = cgi.escape(urllib.unquote(self.path))
        f.write(&apos;&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD HTML 3.2 Final//EN&quot;&gt;&apos;)
        f.write(&quot;&lt;html&gt;\n&lt;title&gt;Directory listing for %s&lt;/title&gt;\n&quot; % displaypath)
        f.write(&quot;&lt;body&gt;\n&lt;h2&gt;Directory listing for %s&lt;/h2&gt;\n&quot; % displaypath)
        f.write(&quot;&lt;hr&gt;\n&quot;)
        f.write(&quot;&lt;form ENCTYPE=\&quot;multipart/form-data\&quot; method=\&quot;post\&quot;&gt;&quot;)
        f.write(&quot;&lt;input name=\&quot;file\&quot; type=\&quot;file\&quot;/&gt;&quot;)
        f.write(&quot;&lt;input type=\&quot;submit\&quot; value=\&quot;upload\&quot;/&gt;&lt;/form&gt;\n&quot;)
        f.write(&quot;&lt;hr&gt;\n&lt;ul&gt;\n&quot;)
        for name in list:
            fullname = os.path.join(path, name)
            displayname = linkname = name
            # Append / for directories or @ for symbolic links
            if os.path.isdir(fullname):
                displayname = name + &quot;/&quot;
                linkname = name + &quot;/&quot;
            if os.path.islink(fullname):
                displayname = name + &quot;@&quot;
                # Note: a link to a directory displays with @ and links with /
            f.write(&apos;&lt;li&gt;&lt;a href=&quot;%s&quot;&gt;%s&lt;/a&gt;\n&apos;
                    % (urllib.quote(linkname), cgi.escape(displayname)))
        f.write(&quot;&lt;/ul&gt;\n&lt;hr&gt;\n&lt;/body&gt;\n&lt;/html&gt;\n&quot;)
        length = f.tell()
        f.seek(0)
        self.send_response(200)
        self.send_header(&quot;Content-type&quot;, &quot;text/html&quot;)
        self.send_header(&quot;Content-Length&quot;, str(length))
        self.end_headers()
        return f

    def translate_path(self, path):
        &quot;&quot;&quot;Translate a /-separated PATH to the local filename syntax.

        Components that mean special things to the local file system
        (e.g. drive or directory names) are ignored.  (XXX They should
        probably be diagnosed.)

        &quot;&quot;&quot;
        # abandon query parameters
        path = path.split(&apos;?&apos;,1)[0]
        path = path.split(&apos;#&apos;,1)[0]
        path = posixpath.normpath(urllib.unquote(path))
        words = path.split(&apos;/&apos;)
        words = filter(None, words)
        path = os.getcwd()
        for word in words:
            drive, word = os.path.splitdrive(word)
            head, word = os.path.split(word)
            if word in (os.curdir, os.pardir): continue
            path = os.path.join(path, word)
        return path

    def copyfile(self, source, outputfile):
        &quot;&quot;&quot;Copy all data between two file objects.

        The SOURCE argument is a file object open for reading
        (or anything with a read() method) and the DESTINATION
        argument is a file object open for writing (or
        anything with a write() method).

        The only reason for overriding this would be to change
        the block size or perhaps to replace newlines by CRLF
        -- note however that this the default server uses this
        to copy binary data as well.

        &quot;&quot;&quot;
        shutil.copyfileobj(source, outputfile)

    def guess_type(self, path):
        &quot;&quot;&quot;Guess the type of a file.

        Argument is a PATH (a filename).

        Return value is a string of the form type/subtype,
        usable for a MIME Content-type header.

        The default implementation looks the file&apos;s extension
        up in the table self.extensions_map, using application/octet-stream
        as a default; however it would be permissible (if
        slow) to look inside the data to make a better guess.

        &quot;&quot;&quot;

        base, ext = posixpath.splitext(path)
        if ext in self.extensions_map:
            return self.extensions_map[ext]
        ext = ext.lower()
        if ext in self.extensions_map:
            return self.extensions_map[ext]
        else:
            return self.extensions_map[&apos;&apos;]

    if not mimetypes.inited:
        mimetypes.init() # try to read system mime.types
    extensions_map = mimetypes.types_map.copy()
    extensions_map.update({
        &apos;&apos;: &apos;application/octet-stream&apos;, # Default
        &apos;.py&apos;: &apos;text/plain&apos;,
        &apos;.c&apos;: &apos;text/plain&apos;,
        &apos;.h&apos;: &apos;text/plain&apos;,
        })


def test(HandlerClass = SimpleHTTPRequestHandler,
         ServerClass = BaseHTTPServer.HTTPServer):
    BaseHTTPServer.test(HandlerClass, ServerClass)

if __name__ == &apos;__main__&apos;:
    test()
</pre>
