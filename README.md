# Introduction

This repository contains the reference architecture for the Effortless Audit patterns. It will provide an opinionated repository structure and usage methodology for the pattern, alongside documentation on the motivations for this pattern and its typical usage.

Please contact Jon Cowie (jcowie@chef.io) or John Snow (jsnow@chef.io) if you have questions or would like more information!

# Effortless Audit Patterns

In this repository, we include two patterns for the Effortless Audit model.

## Automate Pattern

The Automate pattern for Effortless Audit comprises a Habitat application which pulls any InSpec profiles you wish to use from your Automate server. This is most likely going to be the pattern you will start off with when using Effortless Audit, and is particularly suited for cases where the Automate server will be the source of truth for InSpec profiles. It will report the results of any InSpec runs to your Automate server.

## Profile App Pattern

The Profile App pattern for Effortless Audit comprises a Habitat application which contains the InSpec profile to be used inside the app itself. This is in contrast to the Automate pattern in which profiles are downloaded from the Automate server.

This pattern is particularly useful for cases in which you may have an existing bespoke InSpec profile development workflow, or where you do not consider the Automate server to be the source of truth for your InSpec profiles.

It's important to note that although this pattern bundles the InSpec profile with the app, it will still report the status of any InSpec runs to a configured Automate server.

## Windows Profile App Pattern

The Windows Profile App pattern for Effortless Audit is a the same as the Profile App pattern described above, but for Windows.

# Building Effortless Audit Apps

## Requirements

To configure and build a Effortless Audit application, you will need Habitat installed and configured on your development workstation. If you want to upload the package to Habitat Builder, you'll also need to have configured an origin and downloaded its keys into your local Habitat key cache.

You can find instructions on how to work with Habitat Builder [here](https://www.habitat.sh/docs/using-builder/)

## Directory Structure

Before we build the app, this section will outline the contents of the directories that comprise it.

* a2_pattern: This directory contains the Automate pattern for Effortless Audit
* profile_app_pattern: This directory contains the profile app pattern for Effortless Audit
* win_profile_app_pattern: This directory contains the Windows profile app pattern for Effortless Audit

## Building Effortless Audit (Automate Pattern)

To build the Automate pattern app, the only changes needed are as follows:

* In the Habitat plan file (under the `habitat` directory, or `habitat-win/` for Windows builds), edit the package name and package origin you would like to use.
* In the default.toml file (under the `habitat/` directory, or `habitat-win/` for Windows builds), edit the "profiles" line to reflect the InSpec profiles you want to run, and the "reporter" and "compliance" sections to reflect the details of the Automate server your application will report results to.

Once you have completed the above steps, the process for building this application is the same as any other Habitat application:

(please note lines beginning ```[1][default:/src:0]#``` indicate commands run inside hab studio - this portion of the line should not be typed. For Windows builds, you should cd into the `habitat-win/` directory before running the build command)

```
$> hab studio enter
[1][default:/src:0]# build
```

Once the build process has successfully completed, you should see lines similar to the following at the end of the build output:

```
   compliance: Source Path: /src
   compliance: Installed Path: /hab/pkgs/jonlives/compliance/0.1.0/20190116131348
   compliance: Artifact: /src/results/jonlives-compliance-0.1.0-20190116131348-x86_64-linux.hart
   compliance: Build Report: /src/results/last_build.env
   compliance: SHA256 Checksum: a5abe6dde45baf4db5e4441d3bd6defa2f77fd372c634aa366c431ce854c86b4
   compliance: Blake2b Checksum: e1074c38b79ac3d61f46d7db595ef0bf0567b6d6e8818c1f86d6c0c153496a52
   compliance:
   compliance: I love it when a plan.sh comes together.
   compliance:
   compliance: Build time: 0m21s
```

## Building Effortless Audit (Profile App Pattern)

The Profile app pattern makes use of the built-in Habitat integration in InSpec to build a Habitat plan around an existing InSpec profile. The profile_app_pattern directory contains an example of this plan generated against the linux-baseline profile, but full documentation on how to use this integration can be found [here](https://www.inspec.io/docs/reference/habitat/)

Once you have generated the Habitat plan for your InSpec profile, you will need to copy the following files from the ```profile_app_pattern``` directory into the same directory path (create missing directories if necessary) under your new profile:

* habitat/hooks/run
* habitat/config/inspec.json
* habitat/config/settings.sh

Once you have copied these files, the process for building this application is the same as any other Habitat application:

(please note lines beginning ```[1][default:/src:0]#``` indicate commands run inside hab studio - this portion of the line should not be typed.)
```
$> hab studio enter
[1][default:/src:0]# build
```

Once the build process has successfully completed, you should see lines similar to the following at the end of the build output:

```
   linux_baseline: Source Path: /src
   linux_baseline: Installed Path: /hab/pkgs/jonlives/linux_baseline/0.1.0/20190115135231
   linux_baseline: Artifact: /src/results/jonlives-linux_baseline-0.1.0-20190115135231-x86_64-linux.hart
   linux_baseline: Build Report: /src/results/last_build.env
   linux_baseline: SHA256 Checksum: af5c325a4e9998fc3a4ec805b86e15d291605a8b3170fa915214c84d67858db8
   linux_baseline: Blake2b Checksum: 6d477384b05e230807ea0e18a2193601fe65331602b7d3423597ba8a86ec67ce
   linux_baseline:
   linux_baseline: I love it when a plan.sh comes together.
   linux_baseline:
   linux_baseline: Build time: 0m48s
```

The artifact line shows you the path to your newly-build Effortless Audit artifact which can now be uploaded to Habitat Builder or exported to a Docker container to run locally.

## Building Effortless Audit (Windows Profile App Pattern)

The Windows profile app pattern is very similar to the Profile app pattern described above. The win_profile_app_pattern directory contains an example of this plan generated against the windows-baseline profile.

To build this pattern you just need to copy the habitat folder into your profile folder.

Once you have copied these files, the process for building this application is the same as any other Habitat application:

(please note lines beginning ```[HAB-STUDIO] Habitat:\src>``` indicate commands run inside hab studio - this portion of the line should not be typed.)
```
PS C:\effortless_audit\win_profile_app_pattern> hab studio enter
** The Habitat Supervisor has been started in the background.
** Use 'hab svc start' and 'hab svc stop' to start and stop services.
** Use the 'Get-SupervisorLog' command to stream the Supervisor log.
** Use the 'Stop-Supervisor' to terminate the Supervisor.

   hab-studio: Entering Studio at C:\hab\studios\dev--effortless_audit--win_profile_app_pattern
[HAB-STUDIO] Habitat:\src> build
```

Once the build process has successfully completed, you should see lines similar to the following at the end of the build output:

```
   audit-baseline: Source Cache: C:\hab\studios\dev--effortless_audit--win_profile_app_pattern\hab\cache\src\audit-baseline-0.1.0
   audit-baseline: Installed Path: C:\hab\studios\dev--effortless_audit--win_profile_app_pattern\hab\pkgs\thelunaticscripter\audit-baseline\0.1.0\20190606082233
   audit-baseline: Artifact: C:\hab\studios\dev--effortless_audit--win_profile_app_pattern\src\results\thelunaticscripter-audit-baseline-0.1.0-20190606082233-x86_64-windows.hart
   audit-baseline: Build Report: C:\hab\studios\dev--effortless_audit--win_profile_app_pattern\src\results\last_build.ps1
   audit-baseline: SHA256 Checksum: 
   audit-baseline: Blake2b Checksum: 
   audit-baseline: 
   audit-baseline: I love it when a plan.ps1 comes together.
   audit-baseline: 
```

The artifact line shows you the path to your newly-build Effortless Audit artifact which can now be uploaded to Habitat Builder or exported to a Docker container to run locally.
