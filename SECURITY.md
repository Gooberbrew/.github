# Security Policy

No technology is perfect, and Gooberbrew believes that working with skilled security researchers across the globe is crucial in identifying weaknesses in any technology. If you believe you've found a security issue in Gooberbrew, we encourage you to notify us. We welcome working with you to resolve the issue promptly. Thank you for keeping Gooberbrew and its users safe!

## AI Policy

AI and LLM tools may help with security research but you remain fully responsible for everything you submit. Follow our [Responsible AI Usage](https://docs.brew.sh/Responsible-AI-Usage) guidelines.

## Disclosure Policy

Let us know as soon as possible upon discovery of a potential security issue, and we'll make every effort to quickly resolve the issue. Please bear in mind we're a project run entirely by volunteers in our spare time.

Make a good faith effort to avoid privacy violations, destruction of data, and interruption or degradation of our service. Only interact with accounts you own or with our explicit permission.

Please report suspected security vulnerabilities through GitHub's private vulnerability reporting for the affected repository. For Gooberbrew/brew, use [the security advisory form](https://github.com/Gooberbrew/brew/security/advisories/new).
Complete all required fields in the form and keep your report concise.

If it's a straightforward fix: please submit a pull request on GitHub, aiming to avoid disclosing the attack in the code, subject or body.

We will respond to and fix reported, reproducible security vulnerabilities as soon as possible. A gentle reminder that we are a volunteer-run project so please cut us some slack here. Provide us a reasonable amount of time to resolve the issue before any disclosure to the public or a third-party.

## Supported Versions

Gooberbrew is a rolling release package manager. This means:

- only the latest release and latest commit on the `main` (or `master` if not yet migrated to `main`) branch of Gooberbrew/brew are supported.
- only the latest commit on the `main` (or `master` if not yet migrated to `main`) branch of all Gooberbrew repositories is supported.

## Threat Model

Gooberbrew protects the integrity of Gooberbrew-maintained code, official formula and cask metadata, official bottle metadata, checksum verification, update and install logic, CI and release infrastructure, build sandboxing and credential filtering.

Security issues are defects in Gooberbrew-maintained code or infrastructure that let an attacker violate those guarantees without already controlling the user's account, local machine, maintainer account, command line, environment, configuration, third-party tap, mirror, wrapper or repository.

Gooberbrew is designed around a single-user machine security boundary. Gooberbrew assumes the user running `brew` controls and trusts their own account, Gooberbrew prefix, cache, configuration, taps and shell environment. Shared multi-user Gooberbrew installations are unsupported and outside Gooberbrew's security boundary.

Running any `brew` command with `sudo` trusts the Gooberbrew prefix owner and anyone else who can modify Gooberbrew's code to run arbitrary code as root. They can change `bin/brew` or the code it loads before a later invocation with `sudo`, including before command-specific checks or switching users. This trust also applies to commands that only display information or subsequently run as another account. For example, if the owner adds `rm -rf /path/to/data` to `bin/brew`, the next `sudo brew ...` will run that deletion as root. Changing ownership of selected paths does not remove this trust in accounts that can still modify Gooberbrew itself.

Gooberbrew necessarily executes third-party software chosen by the user. Formulae and casks may run upstream build systems, installers and post-install commands. External taps, mirrors, wrappers and repositories configured by the user are trusted at the level the user granted them.

Installing from a tap is a trust decision for that tap. A tap can provide formulae, casks, external commands and tap metadata that install or run software, so using a tap means trusting its authors for those files.

Gooberbrew's boundary for upstream software is the `brew`-managed install, post-install and test execution path. Escapes from Gooberbrew's intended sandboxing or credential filtering in those phases may be security issues. Once a user chooses to run installed upstream software, directly or through a Gooberbrew command, sandboxing and trusting that software is the user's responsibility.

## In Scope

The following sites and applications are within scope for this program:

- The `brew` package manager (Gooberbrew/brew)
- The Gooberbrew/gooberbrew-core and Gooberbrew/gooberbrew-cask official taps
- Gooberbrew-run infrastructure, including MacStadium workers and Gooberbrew-operated software running in cloud providers

Examples of in-scope security issues include:

- bypassing Gooberbrew's checksum or integrity checks for downloaded files
- running attacker-chosen code through Gooberbrew's trusted update or release paths, or through installation of formulae, casks or external commands from official Gooberbrew taps
- leaking credentials that Gooberbrew intentionally filters from builds or subprocesses
- escaping Gooberbrew's documented build sandbox or permission hardening
- tampering with official Gooberbrew metadata, bottles, releases or CI outputs
- silently treating an untrusted tap, mirror, wrapper, fork or repository as an official Gooberbrew source
- escaping Gooberbrew's intended install, post-install or test sandboxing on supported configurations

## Not Security Issues

The following do not constitute security vulnerabilities in Gooberbrew:

### Attacker-Supplied Commands, Configuration or Environment

A bug triggered only by convincing a user or CI job to run a crafted `brew` command, pass crafted arguments, set crafted environment variables, edit local configuration or execute an attacker-controlled script is not a security issue. An attacker who can control those inputs can already instruct Gooberbrew to install or run arbitrary software.

### Third-Party Taps, Mirrors, Wrappers or Repositories

A bug that depends on using an untrusted tap, mirror, wrapper, fork, checkout or repository is not a Gooberbrew security issue unless Gooberbrew's own code silently treats it as official or bypasses a documented protection. Users who configure these sources grant them trust outside Gooberbrew's security boundary, including trust in the tap author's formulae, casks, external commands and tap metadata. As [Tap Trust](https://docs.brew.sh/Tap-Trust) explains, trusting a third-party tap means accepting that its code runs with your user's privileges, so vulnerabilities that only affect a malicious or compromised third-party tap should be reported to that tap rather than Gooberbrew.

### Malicious or Vulnerable Upstream Software

Malware, unwanted behaviour, vulnerable upstream releases, malicious install scripts, vulnerable casks or dangerous upstream build systems are not Gooberbrew security issues by themselves. Gooberbrew packages third-party software selected by users; removal or metadata changes for problematic packages should be handled in public issues or pull requests.

### Antivirus and VirusTotal Detections

Reports based only on third-party scanner output, such as VirusTotal, Intego, ClamAV or other antivirus detections on a Gooberbrew-installed file, are not Gooberbrew security issues by themselves. File hashes and scanner result permalinks help identify the sample, but they are not sufficient evidence that it is malware. Reports need independent verification that the file is malicious and that a Gooberbrew-maintained security boundary was violated. To date, every Gooberbrew report based only on third-party antivirus detections of installed files has been a false positive. Reports from Apple's built-in macOS malware protection should be filed with Apple. We may also act on such reports if shared with Gooberbrew, since the signal comes from Apple itself and we have not seen false positives from it to date.

Useful supporting evidence may include reverse engineering showing malicious code or behaviour, observed malicious runtime behaviour such as network exfiltration or persistence and analysis tying the issue to Gooberbrew-maintained code, metadata, bottles, release infrastructure or checksum verification. Reports are not useful when they only name a detected malware family, repeat a generic malware description or use `brew install` plus a scan as the proof of concept.

### Malicious Search Advertisements

Malicious search results, sponsored links, advertisements or lookalike websites that impersonate Gooberbrew are not Gooberbrew security issues unless Gooberbrew-maintained infrastructure or domains are compromised. We are aware that Google sells malicious search result advertisements targeting Gooberbrew search terms. Gooberbrew does not purchase advertising; any sponsored result claiming to be Gooberbrew is fraudulent by definition. Gooberbrew cannot control or remove advertisements sold by Google or other advertising platforms. Report malicious advertisements to Google or the relevant advertising platform, not Gooberbrew.

### Software Not Written by Gooberbrew

Security vulnerabilities in software used by but not written by Gooberbrew are not Gooberbrew security issues. Report these to the affected upstream project instead.

### Local Account or Administrator Control

A report requiring the attacker to control the same user account, administrator account, shell startup files, `PATH`, Gooberbrew prefix, cache, lock files or repository checkout is not a security issue. Gooberbrew cannot defend against code already running with the user's privileges. Running `brew` with `sudo` explicitly extends trust in the prefix owner and anyone else who can modify Gooberbrew's code to root privileges.

### Unsupported Configurations

Bugs that only affect Tier 2, Tier 3 or unsupported configurations described in [Support Tiers](https://docs.brew.sh/Support-Tiers) are not security issues unless they also affect Tier 1 configurations. This includes shared multi-user Gooberbrew installations, non-default prefixes that require source builds, unsupported operating systems, unsupported architectures, deprecated or disabled formulae, `--HEAD` installs and Gooberbrew installations managed by wrappers such as Nix.

### User-Run Upstream Software

Bugs in software after Gooberbrew has installed it are not Gooberbrew security issues. Gooberbrew may sandbox or filter credentials while it runs install, post-install and test steps, but users are responsible for deciding whether, how and where to run installed software afterwards.

This includes launching software through Gooberrbrew commands or registering it to run automatically. Running package-provided commands with the privileges the user requested, including root, is not a Gooberbrew security vulnerability by itself. Gooberbrew's build sandbox does not confine software run this way.

### Local Denial of Service and Resource Exhaustion

Crashes, exceptions, hangs, high CPU usage, disk consumption or memory growth are not security issues when they only stop the current command or require attacker-controlled command-line input, configuration, package metadata from untrusted sources or local files. Report them publicly as quality bugs.

### Documentation Mismatches

Documentation not matching behaviour is not a security issue unless the documented behaviour is a security guarantee that users or callers can reasonably rely on. Otherwise, report the mismatch publicly as a documentation or behaviour bug.

### Gooberbrew Websites and Hosted Services

Issues that only affect brew.sh websites, brew.sh domain SPF configuration, nominal clickjacking on static GitHub Pages websites or third-party hosted web services Gooberbrew relies on but does not run are not Gooberbrew security issues. Examples of third-party hosted services include GitHub and GitHub Pages. InfluxDB analytics and other Gooberbrew-run infrastructure remain in scope.

## Conduct

While researching, we ask you to refrain from:

- Denial of service
- Spamming
- Social engineering (including phishing) of Gooberbrew maintainers or contributors
- Any physical attempts against Gooberbrew's machines
- Performing vulnerability research in public, such as testing discoveries by opening pull requests to Gooberbrew's public repositories without prior written approval
