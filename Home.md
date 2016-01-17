# Welcome to the netdata!

## Documentation

This wiki is the whole of it. Other than the wiki, currently there is the... source code.

I need help documenting everything. If you can help, please open a github issue stating you want to help. I'll be notified.

You should at least read the section **[[Why netdata?]]**. It has a good overview of the key benefits of installing netdata on your systems.

## Support

If you need help, please use the github issues section.

---


# FAQ

## Is it ready?

Software is never ready. There is always something to improve.

**Netdata is stable** though. I use it on production systems without any issues.

## Is it released?

No it is not. It could be though. I need help to support a release.

## Can I code too?

Of course! Fork the repo, adapt as you see fit and create pull requests.
If you want to discuss your plans to code on netdata, open a github issue to start the discussion.

## Do you plan to implement a bigger / better database?

Yes, check the [[Memory Requirements]] page. I also plan to push all the data to dedicated time series databases like OpenTSDB, thus having netdata acting as a collector for them.

## Is there a roadmap?

Well, I plan to do these:

1. Rename all the charts to follow the naming conventions of OpenTSDB (which will probably require a few changes to the plugins API and the default web dashboard).
2. Allow custom dashboards to be defined using JSON files, to avoid writing HTML
3. Allow per chart documentation to be stored in server side JSON files (outside the dashboards and the netdata daemon)
4. Allow creating dashboards from other dashboards (so that complex dashboards can be created)
5. Improve the memory database (possibly using the internal dedupper, LZ4 compression, disk archiving, mirroring it to third party databases, etc).
6. Create more plugins. A lot more plugins.
7. Allow internal plugins to be forked to external processes (this will protect the netdata daemon from plugin crashes, allow different security schemas for each plugin, etc).
8. Document everything (especially the dashboard and plugin support)
