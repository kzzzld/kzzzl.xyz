+++
date = '2026-10-18T14:01:23+03:00'
draft = false
title = 'My NixOS Experience'
+++

Hello. I used NixOS for like one month and made a full configuration for it. Today, I'll give you some points to make your own configuration and tell you things I did wrong when I started.

Firstly, I used and synchronized my configuration in two machines. One is my Desktop PC and one is M2 MacBook Air (with linux-asahi kernel).

## Why use Flakes

Because output is nicer, you can lock packages so you won't get incompatibility when using two machines with one configuration like I did.

## Why use Home Manager

Because we don't want to link things manually when home manager can do it for you.

### How to use Home Manager?

Firstly you should import it in your flake.nix.

```nix
inputs.home-manager = {
      url = "github:nix-community/home-manager";
      inputs.nixpkgs.follows = "nixpkgs";
};
```

Then use it like this

```nix
{
  home-manager.useGlobalPkgs = true;
  home-manager.useUserPackages = true;
  home-manager.extraSpecialArgs = {inherit inputs;};
  home-manager.backupFileExtension = "backup";
  home-manager.users.kzzzl = import ./home/home.nix;
}
```

Then you can add configuration to ./home/home.nix. You can see my current configuration [here](https://github.com/kzzzld/nixos/tree/43a632eb1d325e65f90341c9e178e00e9601a6d5/home).

## Don't forget to init a git repo

This is really important. Even if it's private, create a GitHub (or GitLab, Codeberg etc.) and push your configuration to it. When you reset your system, you will be able to use that configuration again.

## Make configuration device specific

Even if you are using one machine, split machine configurations. Because when you add another machine, it should be easier to add it. You can split them like this

```
- hosts
  - common
    - configuration.nix
  - nixmac-btw
    - configuration.nix
    - hardware-configuration.nix
  - nixpc-btw
    - configuration.nix
    - hardware-configuration.nix
```


## Don't use AI that much

AI is nice but it can be wrong sometimes. Be sure when you are using it and review the code.
