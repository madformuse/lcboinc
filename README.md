# Lcboinc

Only supports games on Windows at the minute I'm afraid. Will change soon!

## Prerequisites

You will need to have Docker installed, don't know which version but newer is probably better :) I do know it works with Docker Toolbox though (if you change the URL_BASE variable to http://192.168.99.100/)
 
## Building

If you want to be able to build the docker images you will need to install Git LFS as the binaries are installed there. If you don't do this you will only have a pointer in your working copy.

Clone repository (after installing Git LFS) and cd to new directory. Run "docker-compose up -d --build". This will build the neccasary images and start up the containers.

## Creating Work

I haven't yet implemented the automated creation of work so if you want to test game generation you would need to first connect to the container...

docker-compose exec apache bash

su boincadm

Then run one of the following commands. First is Test (T52), second is Main (T40). Each one of these commands causes 2 games to be generated due to 1) BOINCs default preference to create 2 results per WorkUnit for validation and 2) temporarily limited parallelism to 1 on GPUs to prevent accidental short game bias

bin/create_work --appname leela --wu_template templates/test_in --result_template templates/test_out

bin/create_work --appname leela --wu_template templates/main_in --result_template templates/main_out


## Generating Games

Simply connect the BOINC client to http://127.0.0.1/lcboinc (might have to wait a minute when you've started it) and it should behave like any other project, downloading the neccassary files and running the tasks you created.

At the moment it will log into lczero.org as user "boinc". I'm planning to change this to a more identifiable (preferably user specified) name.

## Stopping Containers

In the same directory (once you've exited from bash) just run "docker-compose down -v". The -v also removes the data volumes so you'll be starting with a blank slate next time. If you want to keep your existing mysql data then just omit that option.

## Admin Page

There is an administrative page which you can browse to to query the database to see what's going on. This is located at http://127.0.0.1/lcboinc_ops before you can access it though, you will need to run the following command to setup a username / password for access.

cd html/ops
htpasswd -c .htpasswd username

## Why tom.exe?!

Am I the only one who does that? :) The client needed 2 modifications to work with BOINC as I'm doing it now. Firstly I needed the ability to limit how many games were generated (or we'd only ever have 1 task). Secondly, as BOINC insists that all files with the same name are identical it didn't take very kindly to different versions of lc0.exe to support BLAS or Cuda backends. For this reason I added the ability to provide the name of the engine on the command line. One of the next tasks on my list is to formalize my changes and submit a pull request via the offical channels so I can replace this tom.exe rubbish with client.exe. I've spoken to Crem and Tilps about my changed and they don't think it's a problem.

## Progress

Maybe it would be easier to list the features :) 

So far it's very limited in what you can do. You have to manually create the work, it uses a generic account on lczero, it does not take advantage of parallelism (so it's not very efficient), it only works on Windows, no OpenCL support, no Mac support, it's not secure.

There are some good things to look forward to though! It automatically downloads the correct software for the backend you can support (currently CPU or Cuda), runs one instance of the client per GPU or CPU core (typically one or the other), you can take advantage of BOINC features like only running when idle. In the future I would also like to show some graphics while it's generating games but I'm pretty sure this would mean rewriting the client, so it's not at the top of my list. More important is automatic work generation, parallelism, other OS support, game batches instead of single games for efficiency, linking time limit to next net creation on lczero.org, credit assignment, security, load testing etc etc.

## Contributing

This project is still in it's very early stages so I'm not really looking for code / project support at the moment. I am looking for ideas / concerns though and would welcome any feedback (or guidance on my approach as I am no expert). I'm madformuse on Discord as well if you want to catch up.
