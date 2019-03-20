# Email-error-logs-to-yourself


### About ###
Instead of displaying errors on website, why not using a custom error handler to email error logs to yourself? 

### Usage ###

See the `index.php` file, or use this below:

```php
<?php

// Our custom error handler
function custom_error_handler($number, $message, $file, $line, $vars){
    $email = "
        <p>An error ($number) occurred on line 
        <strong>$line</strong> and in the <strong>file: $file.</strong> 
        <p> $message </p>";
        
    $email .= "<pre>" . print_r($vars, 1) . "</pre>";
    
    $headers = 'Content-type: text/html; charset=iso-8859-1' . "\r\n";
    
    // Email the error to someone...
    error_log($email, 1, 'you@youremail.com', $headers);

    // Make sure that you decide how to respond to errors (on the user's side)
    // Either echo an error message, or kill the entire project. Up to you...
    // The code below ensures that we only "die" if the error was more than
    // just a NOTICE. 
    if ( ($number !== E_NOTICE) && ($number < 2048) ) {
        die("There was an error. Please try again later.");
    }
}

// We should use our custom function to handle errors.
set_error_handler('custom_error_handler');

```
