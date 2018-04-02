## Getting Gerrit reviews

We can use Perceval to retrieve reviews in a [Gerrit](https://www.gerritcodereview.com/) instance. As usual, we can start by asking Perceval for some help:

```bash
(perceval) $ perceval gerrit --help
[2018-04-03 01:04:23,706] - Sir Perceval is on his quest.
usage: perceval [-h] [--category CATEGORY] [--tag TAG] [--from-date FROM_DATE]
                [--archive-path ARCHIVE_PATH] [--no-archive] [--fetch-archive]
                [--archived-since ARCHIVED_SINCE] [-o OUTFILE] [--user USER]
                [--max-reviews MAX_REVIEWS]
                [--blacklist-reviews [BLACKLIST_REVIEWS [BLACKLIST_REVIEWS ...]]]
                [--disable-host-key-check] [--ssh-port PORT]
                url
...
```

From the banner it produces, we learn that the most simple usage is specifying the url for the Gerrit instance, and the user to access it. The Perceval backend uses the Gerrit ssh interface, and thus to use it, ssh access to Gerrit is needed. In most projects, this is granted with not too much trouble, since it is needed to contribute patches. Look for instructions on how to configure ssh for Gerrit, for the specific project to mine. The actual mechanism usually involves signing in to the Gerrit web interface, and then uploading a ssh public key using the "Settings" option in the Gerrit web interface. As an example, see the [instructions on how to get ssh access to the OPNFV Gerrit instance](https://gerrit.opnfv.org/gerrit/Documentation/user-upload.html#ssh).

Once you have access to the Gerrit ssh interface, you only need to specify its url (in fact, the name of the host letting Gerrit ssh access), and the user with granted access. For example, for OPNFV, these would be:

```bash
$ perceval gerrit --user username gerrit.opnfv.org
```

If everything works as intended, the result will be similar to:

```bash
[2018-04-03 01:00:59,271] - Sir Perceval is on his quest.
X11 forwarding request failed on channel 0
X11 forwarding request failed on channel 0
{
    "backend_name": "Gerrit",
    "backend_version": "0.10.2",
    "category": "review",
    "data": {
        "branch": "master",
        "comments": [
            {
                "message": "Uploaded patch set 1.",
                "reviewer": {
                    "email": "jenkins-opnfv-ci@opnfv.org",
                    "name": "jenkins-ci",
                    "username": "jenkins-ci"
                },
                "timestamp": 1522705003
            },
            {
                "message": "Patch Set 1:\n\nBuild Started https://build.opnfv.org/ci/job/opnfv-lint-verify-master/8764/ (1/3)",
                "reviewer": {
                    "email": "jenkins-opnfv-ci@opnfv.org",
                    "name": "jenkins-ci",
                    "username": "jenkins-ci"
                },
                "timestamp": 1522705008
            },
...
[2018-04-03 01:02:07,369] - Received 500 reviews in 13.58s
X11 forwarding request failed on channel 0
...
```

As you can see, reviews are obtained by default in batches of 500. You can control this with `--max-reviews`.

If the ssh interface to Gerrit is not in the default port (29418), you can specify it with `--ssh-port`.