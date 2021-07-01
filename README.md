#!/bin/bash

# Install nginx Partials
#
# Based on https://github.com/webrgp & https://github.com/nystudio107/nginx-craft
#
# @license   MIT

# Exit immediately if a simple command exits with a non-zero status (https://ss64.com/bash/set.html)
set -e

# Helper functions
error_msg () { echo -e "\e[1;31m*** Error:\e[0m $1"; }
success_msg () { echo -e "\e[32m*** $1\e[0m"; }
info_msg () { echo -e "\e[34m*** $1\e[0m"; }
print_msg () { echo -e ">> $1"; }


# Install the nginx partials from https://github.com/astuteo-llc/astuteo-ngnix
install_astuteo_partials () {
if [ ! -d "/etc/nginx/astuteo-partials" ]; then
print_msg "Installing astuteo-partials"
git clone https://github.com/astuteo-llc/astuteo-ngnix.git astuteo-nginx;
cp -R astuteo-nginx/astuteo-partials /etc/nginx;
rm -rf astuteo-nginx;
fi

    success_msg "astuteo-partials installed!"
}


perform_partials_server_setup () {
    # Check if the user is root
    user="$(id -un 2>/dev/null || true)"
    if [ "$user" != 'root' ]; then
        error_msg 'this installer needs the ability to run commands as root.
        We are unable to find either "sudo" or "su" available to make this happen.'
        exit 1
    fi

    success_msg "User is root"

    # Install Steps
    install_astuteo_partials

    success_msg "Astuteo Partials completed!"
}

info_msg "Start Astuteo NGNIX Partials"

perform_partials_server_setup