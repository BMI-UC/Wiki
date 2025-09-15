# Maintaining the Wiki

I [David Lewis](https://github.com/IllustratedMan-code) am the person who
orignally created the wiki. The wiki is a static website created using
[Emanote](https://emanote.srid.ca/).

The wiki was originally a fork of
[emanote-template](https://github.com/srid/emanote-template). Theoretically, as
long as Emanote and github pages still exist, the website should require no
maintenence. However, if Emanote is updated, this wiki will not get those new
features unless certain steps are followed. If you aren't me and feel like you
want to do this, please try to contact me first, as I can perform the updating
steps without much effort.

## Updating Emanote

- These steps require some usage of the [nix](https://nixos.org/) package
  manager. Note that NixOS is not required as nix can be installed in any linux.
- You may be able to use a
  [nix docker container](https://hub.docker.com/r/nixos/nix/) to do this
  maintenence
- You may not need local nix at all, just take a look at the `flake.nix` and the
  `flake.lock` inside the
  [emanote template](https://github.com/srid/emanote-template). Simply copy the
  flake.lock and try to alter the `flake.nix` to make it look like the one in
  the emanote template, while keeping the relavant settings.

1. Examine the `flake.nix` in the
   [emanote template](https://github.com/srid/emanote-template) and make any
   necessary changes
2. Run `nix flake update` in the root directory of the wiki

## Developing features for the wiki

If you want to change the styling or features of the wiki, I can only refer you
to the modification I have done and the emanote documentation/source code. Note
the `templates` directory.
