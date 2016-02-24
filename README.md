# mailmerge
A simple, command line mail merge tool.

By Andrew DeOrio

2016

# Quickstart
1. Install libraries
   ````
   pip install -r requirements.txt
   ````

1. Create a sample template email and database.
    ```
    ./mailmerge.py --sample
    ```

1. Edit the template email and database
    ```
    mailmerge_template.txt
    mailmerge_database.csv
    ```

1. Dry run one email message
    ```
    ./mailmerge.py
      OR
    ./mailmerge.py --limit 1 --dry-run

    ```

1. Dry run all email messages
    ```
    ./mailmerge.py --no-limit
      OR
    ./mailmerge.py --no-limit --dry-run

    ```

1. Send the emails
    ```
    ./mailmerge.py --no-limit --no-dry-run
    ```


# Install
Optionally, use a virtual environment
```
virtualenv venv
. ./venv/bin/activate
```

Install libraries
```
pip install -r requirements.txt
```

# Example
Create a sample template email and database.
```
./mailmerge.py --sample
```

Edit the template email message `mailmerge_template.txt`.  Take note that `TO`, `SUBJECT`, and `FROM` fields are required.  The remainder is the body of the message.  Use `{{ }}` to indicate customized parameters that will be read from the database.  For example, `{{email}}` will be filled in from the `email` column of `mailmerge_database.csv`.
```
TO: {{email}}
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, {{name}},

Your position is {{position}}.

AWD
```

Edit the database `mailmerge_database.csv`.  Notice that the first line is a header that matches the parameters in the template example, for example, `{{email}}`.

**HINT:** Add yourself as the first recipient.  This is helpful for testing.
```
email,name,position
awdeorio@gmail.com,"Drew DeOrio",17
awdeorio@gmail.com,"Drew DeOrio",18
```

Do a dry run of one email message.  This will fill in the template fields of the first email message and print it to the terminal.
```
./mailmerge.py --dry-run --limit 1
>>> message 0
TO: awdeorio@gmail.com
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, Drew DeOrio,

Your position is 17.

AWD

>>> sent message DRY RUN
>>> Limit was 1 messages.  To remove the limit, use the --no-limit option.
>>> This was a dry run.  To send messages, use the --no-dry-run option.
```

If this looks correct, try a second dry run, this time with all recipients using the `--no-limit` option.
```
./mailmerge.py --dry-run --no-limit
>>> message 0
TO: awdeorio@gmail.com
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, Drew DeOrio,

Your position is 17.

AWD

>>> sent message DRY RUN
>>> message 1
TO: awdeorio@gmail.com
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, Drew DeOrio,

Your position is 18.

AWD

>>> sent message DRY RUN
>>> This was a dry run.  To send messages, use the --no-dry-run option.
```

We're being extra careful in this example to avoid sending people spam, so next we'll send one **real** email message.  Recall that you added yourself as the first email recipient.
```
$ ./mailmerge.py --no-dry-run --limit 1
>>> message 0
TO: awdeorio@gmail.com
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, Drew DeOrio,

Your position is 17.

AWD

>>> sent message
>>> Limit was 1 messages.  To remove the limit, use the --no-limit option.
```

Now, check your email make sure the message went through.  If everything looks OK, then it's time to send all the messages.
```
./mailmerge.py --no-dry-run --no-limit
>>> message 0
TO: awdeorio@gmail.com
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, Drew DeOrio,

Your position is 17.

AWD

>>> sent message
>>> message 1
TO: awdeorio@gmail.com
SUBJECT: Testing mailmerge
FROM: Drew DeOrio <awdeorio@gmail.com>

Hi, Drew DeOrio,

Your position is 18.

AWD

>>> sent message
```

# Todo
* Remove my email address from sample
* Complex example in README (e.g., student grade email with optional warning)

  Reference [jinja2 template engine](http://jinja.pocoo.org/docs/latest/templates/)
* Make yourself the first recipient in sample files
* Sanity check `sendmail` executable
* `--output` option for log file
* [Python package](http://peterdowns.com/posts/first-time-with-pypi.html)
* tests
