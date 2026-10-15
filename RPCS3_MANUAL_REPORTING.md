# RPCS3 manual reporting build

Base: official Firebase Apple SDK 13.0.1, `1c35b9453868cea1ecdf9f112b5b23b8b6343854`.

This branch removes the three native handler installation calls from context initialization
and the delayed native-handler inspection. It never installs Mach exception ports, POSIX
signal handlers, Objective-C exception handlers, or a C++ terminate handler. Real debugger
state is still observed unchanged. No handlers are installed and then removed.

Public non-fatal `record(error:userInfo:)`, custom exceptions, custom keys, local persistence,
Firebase transport and collection preferences retain their official implementations.
`FIRCrashlyticsManualReportingOnly()` is an exported marker required by RPCS3 before
Firebase configuration; an accidental stock SDK substitution must fail to link.

Native process crashes and out-of-memory terminations are deliberately not captured.
RPCS3 explicitly submits delivered game fatals and rejected game boots as non-fatal events.

Do not merge the official default branch over this branch without repeating the native
fault-ownership audit and the RPCS3 project reporting tests. The project pins an immutable
commit of this branch, independently of upstream SDK tags.
