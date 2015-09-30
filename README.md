# burp-exceptions
Simple trick to increase readability of exceptions raised by Burp extensions written in Python.
## Rationale
Have you ever written a Burp Extender extension in Python to end up with a completely unlegible exception stack trace? Something like:

    java.lang.RuntimeException: org.python.core.PyException
      at burp.fl.a(Unknown Source)
      at burp.edd.a(Unknown Source)
      at burp.e2g.a(Unknown Source)
      at burp.e2g.g(Unknown Source)
      at burp.i1c.stateChanged(Unknown Source)
      at javax.swing.JTabbedPane.fireStateChanged(JTabbedPane.java:416)
      at javax.swing.JTabbedPane$ModelListener.stateChanged(JTabbedPane.java:270)
      ...
      
Would you rather see something you can actually understand? Just like:

    *** PYTHON EXCEPTION
    Traceback (most recent call last):
      File "/Users/mb/Desktop/burp extension/exceptions_fix.py", line 8, in decorated_function
        return original_function(*args, **kwargs)
      File "/Users/mb/Desktop/burp extension/CustomEditorTab.py", line 78, in setMessage
        self._txtInput.setEsditable(self._editable)
    AttributeError: 'burp.ul' object has no attribute 'setEsditable'

I'm presenting a neat solution to the problem!

## Quick guide

1. Grab https://github.com/securityMB/burp-exceptions/blob/master/exceptions_fix.py and save it to your _Folder for loading modules_. 
   **Hint**: If you don't know what folder it is, open your Burp and go to *Extender*→*Options*→*Python Environment*→*Folder for loading modules*. If you haven't picked any folder yet, just set it now.

2. Modify your Burp extension a bit. Firstly, add the following lines in the beginning of your Python file:

    ```python
from exceptions_fix import FixBurpExceptions
import sys
```
    
  then find your ```registerExtenderCallbacks``` method and add one line to it:
  
    ```python
sys.stdout = callbacks.getStdout()
    ```
    
  and then also add one new line at the very end of the file:

    ```python
FixBurpExceptions()
    ```

3. If you're not sure about the changes you're supposed to make, grab https://github.com/securityMB/burp-exceptions/blob/master/AlteredCustomEditorTab.py and look for lines with ```# ADDED LINE``` comment.

4. Just let the magic happen.

## How does it work?

Details to come...
