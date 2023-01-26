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


## Dev Notes

### 20230125 - Going Live!

I've got some sample articles ready to go live and now I just need publish it.

1.) Generate the output with ??
2.) Upload to AWS with ??
3.) Configure AWS bucket to allow public access.



### 20230124 - First Starting

Created the first steps and started to look into how to intgrate Google Analytics into the website. Appears that I will
need to use a theme. Because of this, copied the theme `simple` to the template directory.
