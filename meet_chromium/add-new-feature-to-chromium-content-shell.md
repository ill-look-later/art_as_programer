### How to Add New Features (without bloating RenderView/RenderViewHost/WebContents)

**<font size="4">Problem</font>**


Historically, new features (i.e. autofill to pick an example) have been added by bolting on their code to 

<font face="'courier new', monospace">RenderView</font>

<font face="arial, sans-serif"> </font>in the renderer process, and 

<font face="'courier new', monospace">RenderViewHost</font>

<font face="arial, sans-serif"> </font>in the browser process. If a feature was handled on the IO thread in the browser process, then its IPC messages were dispatched in 

<font face="'courier new', monospace">BrowserMessageFilter</font>. 

<font face="'courier new', monospace">RenderViewHost</font>

<font face="arial, sans-serif"> </font>would often dispatch the IPC message only to call 

<font face="'courier new', monospace">WebContents</font>

<font face="arial, sans-serif"> </font>(through the 

<font face="'courier new', monospace">RenderViewHostDelegate</font>

<font face="arial, sans-serif"> </font>interface), which would then call to another piece of code. All the IPC messages between the browser and renderer were declared in a massive 

<font face="'courier new', monospace">render_messages_internal.h</font>.  Touching each of these files for every feature meant that the classes became bloated.


**<font size="4">Solution</font>**


We have added helper classes and mechanisms for filtering IPC messages on each of the above threads. This makes it easier to write self contained features.


<font size="3"><b>Renderer side:</b></font>


If you want to filter and send IPC messages, implement the 

<font face="'courier new', monospace">RenderViewObserver</font>

<font face="arial, sans-serif"> </font>interface (

<font face="'courier new', monospace">content/renderer/render_view_observer.h)</font>. The 

<font face="'courier new', monospace">RenderViewObserver</font>

<font face="arial, sans-serif"> </font>base class takes a 

<font face="'courier new', monospace">RenderView</font>

<font face="arial, sans-serif"> </font>and manages the object's lifetime so that it's tied to 

<font face="'courier new', monospace">RenderView</font>

<font face="arial, sans-serif"> </font>(this is overridable). The class can then filter and send IPC messages, and additionally gets notification about frame loading and closing, which many features need.  As an example, see 

<font face="'courier new', monospace">ChromeExtensionHelper&nbsp;</font>

<font face="arial, sans-serif">(</font>

<font face="'courier new', monospace">chrome/renderer/extensions/chrome_extension_helper.h)</font>.


If your feature has part of the code in WebKit, avoid having callbacks go through 

<font face="'courier new', monospace">WebViewClient</font>

<font face="arial, sans-serif"> </font>interface so that we don't bloat it. Consider creating a new WebKit interface that the WebKit code calls, and have the renderer side class implement it. As an example, see 

<font face="'courier new', monospace">WebAutoFillClient</font>

<font face="arial, sans-serif">&nbsp;(</font>

<font face="'courier new', monospace">WebKit/chromium/public/WebAutoFillClient.h</font>).


<font size="3"><b>Browser UI thread:</b></font>


The 

<font face="'courier new', monospace">WebContentsObserver</font>

<font face="arial, sans-serif"> (</font>

<font face="'courier new', monospace">content/public/browser/web_contents_observer.h</font>) interface allows objects on the UI thread to filter IPC messages and also gives notifications about frame navigation. As an example, se

<font size="2">e&nbsp;</font><font face="courier new, monospace" size="2">TabHelper</font><font face="arial, sans-serif"> (</font><font face="'courier new', monospace">chrome/browser/extensions/tab_helper.h</font><font face="arial, sans-serif">).</font>


**<font size="3">Browser other threads:</font>**


To filter and send IPC messages on other browser threads, such as IO/FILE/WEBKIT etc, implement 

<font face="'courier new', monospace">BrowserMessageFilter</font>

<font face="arial, sans-serif"> </font>interface (

<font face="'courier new', monospace">content/browser/browser_message_filter.h</font>). The 

<font face="'courier new', monospace">BrowserRenderProcessHost</font> object creates and adds the filters in its CreateMessageFilters function.


In general, if a feature has more than a few IPC messages, they should be moved into a separate file (i.e. not be added to 

<font face="'courier new', monospace">render_messages_internal.h</font>). This also helps with filtering messages on a thread other than the IO thread. As an example, see 

<font face="'courier new', monospace">content/common/pepper_file_messages.h</font>. This allows their filter, 

<font face="'courier new', monospace">PepperFileMessageFilter</font>, to easily send the messages to the file thread without having to specify their IDs in multiple places.

<pre>
void PepperFileMessageFilter::OverrideThreadForMessage(
    const IPC::Message& message,
    BrowserThread::ID* thread) {
  if (IPC_MESSAGE_CLASS(message) == PepperFileMsgStart)
    *thread = BrowserThread::FILE;
}
</pre>

<pre>

</pre>