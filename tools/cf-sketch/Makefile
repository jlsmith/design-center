REPO:=/var/tmp/cfsketch
DST:=/usr/local/bin

test-defaults:
	./cf-sketch.pl --deactivate-all
	./cf-sketch.pl --install VCS::vcs_mirror --install Monitoring::nagios_plugin_agent
	./cf-sketch.pl --activate VCS::vcs_mirror=../../sketches/utilities/vcs_mirror/params/cfengine-copbl.json
	./cf-sketch.pl --generate -v
	(test -f /var/cfengine/inputs/cf-sketch-runfile.cf && cf-agent -I -K -f /var/cfengine/inputs/cf-sketch-runfile.cf) || cf-agent -I -K -f /var/cfengine/masterfiles/cf-sketch-runfile.cf
	./cf-sketch.pl -s all
	./cf-sketch.pl -l all

test: prep
	./cf-sketch.pl --deactivate-all
	./cf-sketch.pl --install-target=$(REPO) --install=Misc::mysketch -v -f
	./cf-sketch.pl --repolist=$(REPO) --activate Misc::mysketch=./params/mysketch.json -v -f
	./cf-sketch.pl --repolist=$(REPO) --generate -v --runfile ./runme.cf
	cat runme.cf
	cf-agent -I -K -f ./runme.cf

install:
	/usr/bin/install ./cf-sketch.pl $(DST)/cf-sketch

# Eliminate all signs of cf-sketch from the system. For testing purposes.
clean_system:
	/bin/rm -rf /var/cfengine/inputs/sketches /var/cfengine/masterfiles/sketches /etc/cf-sketch $(HOME)/.cf-sketch $(DST)/cf-sketch
prep:
	/bin/mkdir -p $(REPO)
