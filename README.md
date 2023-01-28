# Herrera Labs

Website can be found at [this link](www.anthony-herrera.com)

This is the code for the website that I hope to get started. This is built off 
Python and Pelican which will allow me to create a static website.

## Running The Website

Run the website with this: 

```
pelican content -s pelicanconf.py -t ./themes/notmyidea/ -r -l
```

## Deploying the website

The website can be deployed with the following commands:

```
pelican content -s pelicanconf.py -t ./themes/notmyidea/
aws s3 cp ./output/ s3://anthony-herrera.com --recursive
```

Getting AWS setup to redirect web pages to the bucket is a bit more complicated, but certainly not 
too hard. Will not be covering that here because that is a one time setup.


## Dev Notes

One thing I like to do is keep track of the work done on a project. One of doing that is to create
a dev note section that acts as a fun little diary of that project. It also helps to look back 
when starting new projects to see where other projects failed and how to avoid that the next
time.


### 20230128 - We're Live! Setting up deployment

Somethings in programming there are sentences that feel completely made up or foreign. After waiting for two days for 
the DNS servers to propagate the route53 record updates to point to the new name servers, I got bored and just rolled
with the CloudFLare setup that I had done previously. See that sentence? Wild. Less technical - didn't work after two
days so I just pointed the existing config to the new location. And it worked. Not only did it work, it appears to have
given me the following: 

* DDOS protection
* better security for the S3 bucket where the site is hosted
* SSL certification and analytics. 
* bot blocking / challenging
* Easy region restricting
* site caching
* and a lot more

Accidentally. So that's amazing. Because I swapped the DNS records twice, it's possible that the records are still propagating and will have some "instability" in the near future. Oh well, not thinking about it too hard.

After getting that working, the next order of business is to get the S3 file upload working. Since this is not a website
with a back and front end, I need a way to update files and make changes propagate. At the top of this document it shows
how to do that, but I'm lazy. I don't want to have to run two commands every time I need to update the website.




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
