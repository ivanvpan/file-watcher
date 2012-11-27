#!/usr/bin/env node

var watch = require('watch');
var spawn = require('child_process').spawn;

var args = process.argv.slice(2);

if (args.length < 2) {
    console.log('Usage: watch <extension> <command>');
    process.exit(2);
}

var extension = args[0]
var cmd = args[1];
var cmdArgs = args.slice(2);

var run_cmd = function() {
    var proc = spawn(cmd, cmdArgs);
    /*
    proc.stdout.on('data', function (data) {
      console.log('stdout: ' + data);
    });
    */

    proc.stderr.on('data', function (data) {
      console.log('stderr: ' + data);
    });

    proc.on('exit', function (code) {
        if (code === 0) {
            console.log('Reran command');
        } else {
            console.log('Command failed');
        }
    });
};

watch.createMonitor('.', function (monitor) {
    var re = new RegExp('.*\.' + extension + '$');
    var cb = function(f) {
        if (re.test(f)) {
            console.log('Changed: ' + f);
            run_cmd();
        }
    };

    monitor.on("changed", cb);
    monitor.on("created", cb);
    monitor.on("removed", cb);
});
