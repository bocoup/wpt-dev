# Windows Installation

The following steps are required to run the web platform tests via webdriver
with wptrunner.

## Installation

These largely mirror the steps in the ansible provisioning, but include links
to Windows-specific binaries and provide additional context.

### Global Steps

These steps apply no matter which browser you want to run with webdriver.

- Install OpenSSL 1.1.0 binary from
  [https://slproweb.com/products/Win32OpenSSL.html](https://slproweb.com/products/Win32OpenSSL.html)
  - Either the "Light" or full version will work. Your choice.
- Add `C:\OpenSSL-Win64\bin\openssl.exe` to your system path
  - (Advanced Settings > Environment Variables from Control Panel\All Control
    Panel Items\System).
- Clone [Web Platform Tests](https://github.com/w3c/web-platform-tests) using
  the `--recursive` option
  - e.g. `git clone --recursive git@github.com:w3c/web-platform-tests.git`
  - Doing `git submodule update --init --recursive` once the repo was checked
    out can result in bad checkouts.
- Update submodules: `git submodule update --init --recursive`
- Edit hostfile (C:\Windows\System32\drivers\etc\hosts) to include the
  following lines:
  - ```
    127.0.0.1    web-platform.test
    127.0.0.1    www.web-platform.test
    127.0.0.1    www1.web-platform.test
    127.0.0.1    www2.web-platform.test
    127.0.0.1    xn--n8j6ds53lwwkrqhv28a.web-platform.test
    127.0.0.1    xn--lve-6lad.web-platform.test
    0.0.0.0      nonexistent-origin.web-platform.test
    ```
- Download and Install
  [Ahem](http://www.w3.org/Style/CSS/Test/Fonts/Ahem/AHEM____.TTF) font
- Install Python 2.7. Ensure both C:\Python and C:\Python\Scripts are in your
  system path
  - See the OpenSSL system path instruction above for how to do this.
- Clone [wptrunner](https://github.com/w3c/wptrunner)
- Install wptrunner dependencies (in wptrunner directory, `pip install
  --editable ./`
- Create both a `meta` directory and a `download` directory alongside the
  `wptrunner` and `web-platform-tests` directories
- Create manifest file
  - Change into the `web-platform-tests` directory
  - Run `python manifest`

### Browser: Chrome Canary

The following steps describe how to install Chrome Canary and chromedriver.

- Download and install [Chrome Canary](https://www.google.com/chrome/browser/canary.html)
- Download and unpack the latest
  [Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/downloads)
  `exe` into the `download` folder.
- Install chromedriver requirements. In `wptrunner`: `pip install --requirement
  requirements_chrome.txt`

### Browser: Firefox Nightly

The following steps describe how to install Firefox Nightly, geckodriver, and
the necessary profile.

- Download and install
  [Firefox Nightly(https://www.mozilla.org/en-US/firefox/channel/desktop/)
  - If you have another instance, it is worth adding a new profile in
    `about:profiles` and updating the Nightly shortcut (in
    `C:\ProgramData\Microsoft\Windows\Start Menu\Programs`) to set the target
    to `"C:\Program Files\Nightly\firefox.exe" -p <profile_name> -no-remote`
- Download and upack the latest
  [geckodriver](https://github.com/mozilla/geckodriver/releases) `exe` into the
  `download` folder.
- Install geckdriver requirements. In `wptrunner`: `pip install --requirement
  requirements_firefox.txt`
- Fetch gecko profile
  - Change into `download` directory
  - Create a `gecko-dev` directory inside the `download` directory
  - Change into the `gecko-dev` directory
  - `git init`
  - `git remote add origin git@github.com:mozilla/gecko-dev.git`
  - `git config core.sparsecheckout true`
  - Create a file at `.git/info/sparse-checkout` add the single line
    `testing/profiles/*` to the file and save.
  - `git pull --depth=1 origin master`

### Browser: Microsoft Edge

The following steps describe how to install Microsoft Edge and
MicrosoftWebDriver.

A prerequisite is that you are running a current version of Windows 10.

- Find your current Windows 10 build number.
  - Go to Start > Settings > System > About and find the number listed next to
    "OS Build"
- Download the webdriver corresponding to that build number from
  https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/ into
  the `download` folder.

## Running the Tests

If you installed everything as above, then the following commands should run
the web platform tests. Note that these commands will run *ALL* of the web
platform tests. You may want to
[run a subset](https://github.com/w3c/wptrunner#example-how-to-run-a-subset-of-tests)
of the tests.

### Chrome Canary

Replace <user-name> with your Windows user name

`wptrunner --certutil-binary openssl --tests .\web-platform-tests --metadata
.\web-platform-tests\ --product chrome --binary
"C:\Users\<user-name>\AppData\Local\Google\Chrome SxS\Application\chrome.exe"
--webdriver-binary .\download\chromedriver.exe`

### Firefox

`wptrunner --certutil-binary openssl --tests .\web-platform-tests --metadata
.\web-platform-tests\ --product firefox --binary
"C:\Program Files\Nightly\firefox.exe" --prefs-root
.\downloads\gecko-dev\testing\profiles --webdriver-binary
.\download\geckodriver.exe`

### Edge

Note that this does not require a `--binary` flag. Microsoft Edge is a Windows
App Store-style app that does not have a traditional executable. The webdriver
for your build will know how to invoke the browser.

`wptrunner --certutil-binary openssl --tests .\web-platform-tests --metadata
.\web-platform-tests\ --product edge --webdriver-binary
.\download\MicrosoftWebDriver.exe`
