# Emacs RSpec Mode
RSpec mode provides some convenience functions for dealing with RSpec.

## Installation

You can install via ELPA, or manually by downloading `rspec-mode` and
adding the following to your init file:

```emacs
(add-to-list 'load-path "/path/to/rspec-mode")
(require 'rspec-mode)
```

Provided you have `yasnippet` installed, you can load the snippets:

```emacs
(eval-after-load 'rspec-mode
 '(rspec-install-snippets))
```

(This isn't done automatically to avoid conflicts with snippets you
may already have set up.)

## Usage

If `rspec-mode` is installed properly, it will be started
automatically when `ruby-mode` is started.

Command                                                             | Description                                                                                                               | RSpec Verifiable mode binding
------------------------------------------------------------------- |---------------------------------------------------------------------------------------------------------------------------|------------------------------
<kbd>M-x rspec-verify</kbd>                                         | Verify the spec file associated with the current buffer                                                                   | `C-c , v`
<kbd>M-x rspec-verify-all</kbd>                                     | Run spec for entire project                                                                                               | `C-c , a`
<kbd>M-x rspec-toggle-spec-and-target</kbd>                         | Toggle back and forth between a spec and its target                                                                       | `C-c , t`
<kbd>M-x rspec-toggle-spec-and-target-find-example</kbd>            | Toggle back and forth between a method and its examples in the spec file                                                  | `C-c , e`
<kbd>M-x rspec-find-spec-or-target-other-window</kbd>               | Find in the other window the spec or the target file                                                                      | `C-c , 4 t`
<kbd>M-x rspec-find-spec-or-target-find-example-other-window</kbd>  | Find in the other window the spec or the target file, and try to navigate to the example or method corresponding to point | `C-c , 4 e`
<kbd>M-x rspec-rerun</kbd>                                          | Re-run the last verification process                                                                                      | `C-c , r`
<kbd>M-x rspec-verify-matching</kbd>                                | Run all specs related to the current buffer                                                                               | `C-c , m`
<kbd>M-x rspec-verify-continue</kbd>                                | Run the current spec and all after it                                                                                     | `C-c , c`
<kbd>M-x rspec-verify-method</kbd>                                  | Verify the example or method defined at point                                                                             | `C-c , s`
<kbd>M-x rspec-run-last-failed</kbd>                                | Re-run just the failed examples from the last run                                                                         | `C-c , f`

Command                                         | Description                                    | RSpec mode binding
------------------------------------------------|------------------------------------------------|-------------------
<kbd>M-x rspec-verify-single</kbd>              | Run the specified example at point             | `C-c , s`
<kbd>M-x rspec-toggle-example-pendingness</kbd> | Toggle the pendingness of the example at point | `C-c , d`

Command                                  | Description                                                    | RSpec Dired mode binding
-----------------------------------------|----------------------------------------------------------------|-------------------------
<kbd>M-x rspec-dired-verify</kbd>        | Run all specs in the current directory                         | `C-c , v`
<kbd>M-x rspec-dired-verify-single</kbd> | Run marked specs or spec at point (works with directories too) | `C-c , s`
<kbd>M-x rspec-verify-all</kbd>          | Run the 'spec' rake task for the project of the current file   | `C-c , a`
<kbd>M-x rspec-rerun</kbd>               | Re-run the last RSpec invocation                               | `C-c , r`

See `rspec-mode.el` for further usage.

## Gotchas
### Debugging

To use `binding.pry` or `byebug`, install `inf-ruby` and add this to
your init file:

```emacs
(add-hook 'after-init-hook 'inf-ruby-switch-setup)
```

When you've hit the breakpoint, hit `C-x C-q` to enable `inf-ruby`.

### ZSH and RVM

If you use `ZSH` and `RVM`, you may encounter problems running the
specs. It may be so that an older version of Ruby, than the one you
specified in `.rvmrc`, is used. This is because `ZSH` runs a small
script each time a shell is created, which modifies the `$PATH`. The
problem is that it prepends some default paths, such as `/usr/bin`,
which contains another `ruby` binary.

What you can do to solve this is to use `BASH` for running the
specs. This piece of code does the job:

```emacs
(defadvice rspec-compile (around rspec-compile-around)
  "Use BASH shell for running the specs because of ZSH issues."
  (let ((shell-file-name "/bin/bash"))
    ad-do-it))

(ad-activate 'rspec-compile)
```

### Auto-scrolling

Set `compilation-scroll-output`. For example, `(setq compilation-scroll-output t)`
will turn on auto scrolling.


## Contributing

Love RSpec and Emacs? Great, help out by contributing. The easiest way
to contribute is to checkout the
[git project](https://github.com/pezra/rspec-mode.git), make a change
and then submit a pull request.

### Note on Patches/Pull Requests

 * Fork the project.
 * Make your feature addition or bug fix.
 * Update the version and changelog in the header of rspec-mode.el to
   reflect the change.
 * Send me a pull request. Bonus points for topic branches.
