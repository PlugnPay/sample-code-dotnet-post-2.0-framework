====================================================
PlugnPay .Net Version 2.0 API
created on 09/18/2009
====================================================

***** IMPORTANT NOTES *****
This API is being provided "AS IS".  Limited technical support assistance will
be given to help diagnose/address problems with this API.  The amount of support
provided is up to PlugnPay's staff.

It is recommended if you experience a problem with this API, first seek assistance
through this API's readme file, then check with the MSDN developer forum, and if you
are still unable to resolve the issue, contact us via PlugnPay's Online Helpdesk.

You will required to supply your login username/password for payment processing.
The authorization will be done via PlugnPay's API payment method.  The API is
intended to take advantage of .Net Version 2.0's ability to connect to PlugnPay's 
payment gateway for payment processing,

If you want to change the behavior of this API, please feel free to make changes
to the files yourself.  However, customized API modules will not be provided
support assistance.

***************************



1.  Implementing the API

This API has been written to be a self contained VB.Net function to make implementation of it much easier.  There are two steps which need to be take to us it.  First step is to cut and paste the PnPSend function (found in PnPSend.txt) into your source code.  The function is a protected function which means that you will only be able to call it from the same form as the function is placed.  Once the function is placed you will need to make sure that you have a reference to System.web under your project references.



2.  Using the API

The PnPSend function is called the same was as any other .Net function.  There are two inputs to the function Params and URL.  Params is a string which contains your transaction's information using the standard found on our Remote Client Integration Specification which can be found by going to your administration area and clicking on the Documentation/FAQ link.  The other input is the submission URL, which by default is set to the proper URL, but if you needed to override it for any reason you can, just by entering the new URL in this input.

When you call the function the API will take your string and submit it to our secure server.  The entire process uses SSL encryption so it is done safely.  The API will then return a string containing the servers response and/or an error message if there was a problem.  This API will automatically encode your string before sending it and decode it before returning it back to you.  The only time additional decoding is required would be if you are running a transaction which contains multiple levels of data (such as a Query_trans).  If a transaction does contain multiple levels we will have it's upper levels decoded automatically but each of the lower levels will need to be manually decoded outside of the API (sample source code can be found in section 4b).



3.  Troubleshooting

'HttpUtility' is not a member of 'Web'.
This error is caused if you have not added the reference to System.Web in the reference section of your project.

pnpcom_err=Non HTTPS URL
This means that the custom URL you have entered is not an HTTPS URL and can have the information sent to it.

pnpcom_err=No Params
No string was passed to the API for processing



4.  Sample Code 

4a. Auth Sample Code
The code below will allow you to use the PnPSend function to send a simple auth to our system.  To use this sample you just need to create a new form with two textboxes and a button.  Once you create that you need to add the reference to System.web and paste both the PnPSend function and the sample code below into the form coding section.

    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click
        TextBox2.Clear() 'Clears textbox2
        Dim TempArray = Split(PnPSend(TextBox1.Text), "&") 'Breaks up the results from PnPSend into an array using '&' to determine each field
        Dim i As Integer
        While i < TempArray.length
            TextBox2.Text += TempArray(i) & vbNewLine 'write out each field and a newline
            i += 1
        End While
    End Sub


4b. Query_trans Sample Code
The code below will allow you to use the PnPSend function to send a query trans and to loop through the results and decode them.  To use this sample you just need to create a new form with one textbox, a listbox and a button.  Once you create that you need to add the reference to System.web and paste both the PnPSend function and the sample code below into the form coding section.

    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click
        ListBox1.Items.Clear() 'clears out the listbox
        Dim TempArray = Split(PnPSend(TextBox1.Text), "&") 'Breaks up the results from PnPSend into an array using '&' to determine each field
        Dim i As Integer
        While i < TempArray.length
            ListBox1.Items.Add(System.Web.HttpUtility.UrlDecode(TempArray(i))) 'write out each field and a newline and decode
            i += 1
        End While
    End Sub

