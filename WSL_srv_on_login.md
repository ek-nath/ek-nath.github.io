# Run a Linux service on Windows login using WSL

I want to run a python webserver on boot. This is a local service that I use to store my kanban board.

## Author the startup script

The program that I want to run is: `python -m http.server 8008 -d <path_to_directory>`. When I run this using `wsl` as `wsl -u user -r python -m http.server 8008 -d <path_to_directory>`, it errors out because like `cron` it needs the full path to `python`.

So this works:
`wsl -u user -e /home/user/.pyenv/shims/python -m http.server 8008 -d /.../my_dir`

So, create a `start_service.bat` file with this as the content

## Create the startup entry

1. Run `shell:startup` in the Windows Run command (`Win+R`)
2. Create a shortcut for this `start_service.bat` in this directory
3. Logout and login again to test the service
