# Herrera Labs

This is the code for the website that I hope to get started. This is built off Python and Pelican which will allow me to
create a static website.

## Running The Website

Run the website with this: 

```
pelican content -s pelicanconf.py -t ./themes/notmyidea/ -r -l
```

## Deploying the website

The website can be deployed with the following commands:

```
pelican content -s pelicanconf.py -t ./themes/notmyidea/ -r -l
aws s3 cp ./output/ s3://anthony-herrera.com --recursive
```

Getting AWS setup to redirect web pages to the bucket is a bit more complicated, but certainly not 
too hard. Will not be covering that here because that is a one time setup.


## Dev Notes

One thing I like to do is keep track of the work done on a project. One of doing that is to create
a dev note section that acts as a fun little diary of that project. It also helps to look back 
when starting new projects to see where other projects failed and how to avoid that the next
time.


### 20230126 - Going Live Again!

Created the output directory and uploaded files to AWS S3. Updated documentation to show how to do 
all of that. Now, need to get the site itself ready. The only thing left to do is have the URL
point to the S3 bucket where the files are stored.

### 20230125 - Going Live!

I've got some sample articles ready to go live and now I just need publish it.

1.) Generate the output with ??
2.) Upload to AWS with ??
3.) Configure AWS bucket to allow public access.



### 20230124 - First Starting

Created the first steps and started to look into how to intgrate Google Analytics into the website. Appears that I will
need to use a theme. Because of this, copied the theme `simple` to the template directory.
