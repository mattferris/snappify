Snappify
========

Snappify is used to create and maintain directory snapshots. It includes a number of low-level utilities and a wrapper to add high-level interaction.

Create a snapshot
-----------------

Creating a snapshot is as easy as changing to the directory you want to take a snapshot of and running `snappify init` then `snappify snap`.

    # cd /snapshot/directory
    # snappify init
    initializing snappify repository in /snapshot/directory
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

Restoring a snapshot
--------------------

To restore a snapshot, run `snappify restore` and pass the snapshot ID or a valid alias.

    # snappify restore foo

If no snapshot is specified, the snapshot reference by the `last` alias will be used.

    # ls
    helloWorld.txt        foo.txt
    # snappify snap
    created snapshot 9054fbe0b622c638224d50d20824d2ff6782e308
    # rm helloWorld.txt
    # ls
    foo.txt
    # snappify restore
    restored snapshot 9054fbe0b622c638224d50d20824d2ff6782e308 into /snapshot/directory
    # ls
    helloWorld.txt        foo.txt

Snapshot storage
----------------

Running `snappify init` creates a new directory called `.snappify` in the current directory. All snapshots are stored within the `.snappify` directory. This directory is called the *repository*. Over time, the repository may accumulate a lot of snapshots. To refresh the repository you can run `snappify clean`, which will delete all the snapshots. You can list all the snapshots in the repository by running `snappify list`.
