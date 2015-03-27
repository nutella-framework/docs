# nutella.app.log

This application-level logging API has exaclty the same methods of its run-level counterpart. Refer to [that page](nutella.log.md) for a complete documentation.

The only difference is where the log files are sent to. For application-level APIs, unsurprisingly, logs end up in the application-level logging channel `/nutella/apps/app_id/logging`)