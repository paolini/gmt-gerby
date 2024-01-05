# references

* https://github.com/gerby-project/gerby-website
* https://gerby-project.github.io/stacks-instructions

# setup

create a virtulenv (only once):

```
python -m venv venv
```

activate it:

```
. venv/bin/activate
```

install the dependencies specified in the setup file:

```
pip install .
```

replace the package `mdx_bleach` with a patched version:

```
git clone https://github.com/paolini/mdx_bleach
pip uninstall mdx_bleach
pip install ./mdx_bleach
rm -fr ./mdx_bleach
```

# run locally

```
cd gerby/tools
python3 wsgi.py
```


# systemctl service configuration

In `/etc/systemd/system/gerby.service`

```
[Unit]
#  specifies metadata and dependencies
Description=Gunicorn instance to serve gerby gmt-project
After=network.target
# tells the init system to only start this after the networking target has been reached
# We will give our regular user account ownership of the process since it owns all of the relevant files
[Service]
# Service specify the user and group under which our process will run.
User=root
# give group ownership to the www-data group so that Nginx can communicate easily with the Gunicorn processes.
Group=www-data
# We'll then map out the working directory and set the PATH environmental variable so that the init system knows where our the executables for the process are located (within our virtual environment).
WorkingDirectory=/root/gmt/gerby-website/gerby/tools
Environment="PATH=/root/gmt/gerby-website/gerby/tools"
# We'll then specify the commanded to start the service
ExecStart=/root/gmt/gerby-website/venv/bin/gunicorn --workers 3 --bind unix:app.sock -m 007 wsgi:app
# This will tell systemd what to link this service to if we enable it to start at boot. We want this service to start when the regular multi-user system is up and running:
[Install]
WantedBy=multi-user.target
```