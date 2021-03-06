# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'
require 'yaml'

VAGRANT_API_VERSION = "2"

confDir = $confDir ||= File.expand_path("~/.vbdocker")

vbdockerYamlPath = confDir + "/Vbdocker.yaml"
vbdockerJsonPath = confDir + "/Vbdocker.json"
afterScriptPath = confDir + "/after.sh"
aliasesPath = confDir + "/aliases"


require File.expand_path(File.dirname(__FILE__) + '/scripts/vbdocker.rb')

Vagrant.require_version '>=1.8.4'

Vagrant.configure(VAGRANT_API_VERSION) do |config|
  config.vm.box = "debian/stretch64"
  config.vm.hostname = "vbdocker"

  if File.exists? aliasesPath then
        config.vm.provision "file", source: aliasesPath, destination: "~/.bash_aliases"
    end

    if File.exists? vbdockerYamlPath then
        settings = YAML::load(File.read(vbdockerYamlPath))
    elsif File.exists? vbdockerJsonPath then
        settings = JSON.parse(File.read(vbdockerJsonPath))
    end

    Vbdocker.configure(config, settings)

    if File.exists? afterScriptPath then
        config.vm.provision "shell", path: afterScriptPath, privileged: false
    end

    if defined? VagrantPlugins::HostsUpdater
        config.hostsupdater.aliases = settings['sites'].map { |site| site['map'] }
    end

end
