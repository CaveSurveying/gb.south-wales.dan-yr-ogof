# Dan yr Ogof

The survey of **Dan yr Ogof**, in the Upper Tawe valley, Brecon Beacons: the Cave Registry Data
Archive's **DYOcatchment** Therion project (the DistoX resurvey so far), with its Subversion
history, and the cave in 3D under the Welsh Government's LiDAR, built from it by
[cave-viewer](https://github.com/CaveSurveying/cave-viewer).

**This repository is private, and the site isn't published.** The DYOcatchment data doesn't
state a licence, so nothing built from it goes public until the Cave Registry
(data@cave-registry.org.uk) and the surveyors say it may. The build is kept as each run's
artifact to download and look at instead (below).

## Where the data comes from

This is a copy of the Cave Registry Data Archive's **DYOcatchment** repository, with its
history: every Subversion revision is a commit here, with its author, date and message, and
`SVN-Revision` and `SVN-URL` at the end of the message. **The source of truth is the SVN
repository, <https://www.cave-registry.org.uk/svn/DYOcatchment/>:** that's where the survey is
worked on and where changes go, so don't make survey changes here.

Eight revisions (r5 to r11 and r19, which the log shows with no author or date) are in
folders anonymous users can't read, so there's nothing of them here.

Until the switch to git, a scheduled workflow
([`.github/workflows/svn-sync.yml`](.github/workflows/svn-sync.yml)) brings in each new
revision every day, with [`tools/svn_sync.py`](tools/svn_sync.py), and starts the build when it
brought something in. It fetches only what each revision changed (it needs Subversion's `svn`
client, but no checkout). To bring them in by hand:

```bash
python3 tools/svn_sync.py
```

## The 3D view

Built on every push and pull request ([`.github/workflows/build.yml`](.github/workflows/build.yml),
cave-viewer's shared workflow) from [`caves.json`](caves.json): the Therion project in `DYO/`
(`dyo.th`), on the SN square of the National Grid, under the Welsh Government's 1 m LiDAR, with
the plan Therion draws and the passage walls from the drawings. [`common_layout.th`](common_layout.th)
gives Therion's plan the "north grid" cave-viewer needs to place it.

To look at a build: the run's page in the Actions tab, the **github-pages** artifact (a tar
of the site); unpack it and serve it:

```bash
mkdir site && tar -xf artifact.tar -C site
python3 -m http.server -d site     # then open http://localhost:8000
```

To build it yourself (Therion, Survex and Subversion installed):

```bash
python3 -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt
python -m caveviewer build caves.json --out site/
python -m http.server -d site
```

## The files

| | |
|---|---|
| `DYO/` | The registry's Therion project (`dyo.th`): the survey data and drawings. |
| `caves.json` | cave-viewer's config for the 3D view. |
| `common_layout.th` | Therion's "north grid", for the drawn plan. |
| `requirements.txt` | cave-viewer, pinned to a release. |
| `tools/svn_sync.py` | Brings in the registry's new revisions. |

Set up from the trials in `paperclipmonkey/uk-cave-3d-trials` (regions/wales), where Dan yr Ogof
was first built.
