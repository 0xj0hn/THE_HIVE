# DLL Hijacking

## <mark style="color:red;">DLL Hijacking Techniques</mark>

There is a variety of approaches to choose from, with success depending on how the application is configured to load its required DLLs. Possible approaches include:

<mark style="color:orange;">**DLL replacement:**</mark>** ** replace a legitimate DLL with an evil DLL. This can be combined with DLL Proxying, which ensures all functionality of the original DLL remains intact.

<mark style="color:orange;">**DLL search order hijacking:**</mark> DLLs specified by an application without a path are searched for in fixed locations in a specific order \[3]. Hijacking the search order takes place by putting the evil DLL in a location that is searched in before the actual DLL. This sometimes includes the working directory of the target application.

<mark style="color:orange;">**Phantom DLL hijacking:**</mark> drop an evil DLL in place of a missing/non-existing DLL that a legitimate application tries to load .

<mark style="color:orange;">**DLL redirection:**</mark> change the location in which the DLL is searched for, e.g. by editing the %PATH% environment variable, or .exe.manifest / .exe.local files to include the folder containing the evil DLL.

<mark style="color:orange;">**WinSxS DLL replacement (side-loading) :**</mark>** ** replace the legitimate DLL with the evil DLL in the relevant WinSxS folder of the targeted DLL. Often referred to as DLL side-loading.

<mark style="color:orange;">**Relative path DLL Hijacking:**</mark> copy (and optionally rename) the legitimate application to a user-writeable folder, alongside the evil DLL. In the way this is used, it has similarities with (Signed) Binary Proxy Execution \[8]. A variation of this is (somewhat oxymoronically called) ‘bring your own LOLbin’ in which the legitimate application is brought with the evil DLL (rather than copied from the legitimate location on the victim’s machine).
