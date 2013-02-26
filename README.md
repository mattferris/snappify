Snappify
========

Snappify is used to create and maintain directory snapshots. It includes a number of low-level utilities and a wrapper to add high-level interaction.

Create a snapshot
-----------------

Creating a snapshot is as easy as changing to the directory you want to take a snapshot of and running `snappify init` then `snappify snap`.

    # cd /home/user
    # snappify init
    initializing snappify repository in /home/user/.snappify
    # snappify snap
    created snapshot 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83
    
`4e1243bd22c66e76c2ba9eddc1f91394e57f9f83` is the randomly generated ID of the snapshot as is used to reference the snapshot in other actions. You can create a more friendly reference to the snapshot by creating an alias.

    # snappify alias foo 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83
    foo -> 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83

When creating the snapshot, you can also specify an alias.

    # snappify snap foo
    created snapshot 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83 (foo)

A special alias `last` always references the last snapshot created. `snappify alias` will show you all the aliases.

    # snappify alias
    last -> 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83
    foo -> 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83

A quick way to assign an alias to a snapshot is to specify an alias when creating the snapshot.

    # snappify snap foo
    created snapshot 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83 (foo)
    # snappify alias
    last -> 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83
    foo -> 4e1243bd22c66e76c2ba9eddc1f91394e57f9f83

In lieu of an alias, snapshots can also be referenced by partial IDs. For example, the about snapshot could be referenced as `4e12`. There is no limit to how few characters to use. Be aware, however, that if two snapshots match the partial ID, the newest one will be used. You can use `snappify-alias` to determine which snapshot will be used for a given partial ID.

    # snappify alias 4e12
    4e1243bd22c66e76c2ba9eddc1f91394e57f9f83

Checking for changes
--------------------

A summary of the changes made since the last snapshot can be viewed using `snappify status`. It will display whether files are new (`n`), modified (`m`) or deleted (`d`).

    # rm temp.txt
    # snappify status
    d temp.txt
    # snappify status 7b82
    d temp.txt
    m bar.txt

If no snapshot is specified, `last` is assumed.

You can see the changes that have been made to each file using `snappify diff`. This will produce a diff for each file that has changed. If no snapshot is defined, `last` is assumed.

    # snappify diff
    --- /home/user/.snappify/snaps/1feed73104b827183ac3eddb290012cc3d3c532a/bar.txt  2013-02-23 23:31:04.000000000 -0800
    +++ /home/user/bar.txt  2013-02-25 13:39:11.520759306 -0800
    @@ -0,0 +1,2 @@
    +Hello world!
    +Hello again!

You can see the changes for a specific file by specifying it.

    # snappify diff bar.txt

To see the changes for files since an older snapshot, specify the snapshot.

    # snappify diff 4e12
    ..or..
    # snappify diff 4e12 bar.txt

Restoring a snapshot
--------------------

To restore a snapshot, run `snappify restore` and pass the snapshot ID or a valid alias.

    # snappify restore foo

If no snapshot is specified, the snapshot reference by the `last` alias will be used.

    # ls
    helloWorld.txt        bar.txt
    # snappify snap
    created snapshot 9054fbe0b622c638224d50d20824d2ff6782e308
    # rm helloWorld.txt
    # ls
    bar.txt
    # snappify restore
    restored snapshot 9054fbe0b622c638224d50d20824d2ff6782e308 into /snapshot/directory
    # ls
    helloWorld.txt        bar.txt

Snapshot storage
----------------

Running `snappify init` creates a new directory called `.snappify` in the current directory. All snapshots are stored within the `.snappify` directory. This directory is called the *repository*. Over time, the repository may accumulate a lot of snapshots. To refresh the repository you can run `snappify clean`, which will delete all the snapshots. You can list all the snapshots in the repository by running `snappify list`.

    # snappify list
    2013-02-22.15:02:38 8404516183a4734a7965e0a6fcf829c71f0b0cda /home/user
    2013-02-23.13:38:56 9343d53a70a4be629ffa662b3b6cce3bcbc9005d /home/user
    2013-02-25.17:30:21 1feed73104b827183ac3eddb290012cc3d3c532a /home/user
    # snappify clean
    # snappify list
