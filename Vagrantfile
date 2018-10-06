# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# Importeer config.yml
settings    = YAML.load_file File.dirname(File.expand_path(__FILE__)) + '/config.yml'
virtualbox  = settings['virtualbox']
provision   = settings['folders']['provision']
iis         = settings['iis']
asp         = settings['asp']
sql         = settings['sql']

# Het getal 2 verwijst naar de versie die gebruikt wordt. Niet wijzigen!
Vagrant.configure("2") do |config|

    config.vm.box = "JonasV/WindowsServer2016"

    config.vm.provider "virtualbox" do |vb|
        vb.name = virtualbox['name']
        vb.cpus = virtualbox['cpus']
        vb.memory = virtualbox['memory']
        vb.gui = virtualbox['gui']
        vb.customize ["modifyvm", :id, "--usbxhci", virtualbox['usb3']]
    end

    config.vm.network "private_network", ip: settings['ip-address']

    config.vm.provision :shell,
        path: provision + "/installiis.ps1",
        args: ["-username " + iis['username'] + " -password " + iis['password'] + " -downloadpath  " + iis['downloadpath']]
    config.vm.provision :shell,
        path: provision + "/installasp.ps1",
        args: ["-asp35 " + asp['asp35'] + " -asp45 " + asp['asp45']]
    config.vm.provision :shell,
        privileged: true,
        path: provision + "/installsqlserver.ps1",
        args: ["-downloadpath " + sql['downloadpath'] + " -instancename " + sql['instancename'] + " -rootpassword "+ sql['rootpassword'] + " -tcpportnr " + sql['tcpportnr'] + " -dbname " + sql['dbname'] + " -username " + sql['username'] + " -password " + sql['password']]

end
