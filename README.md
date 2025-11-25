 :: Gentoo Linux Next :: Beta ::
 ==============================================

 ABSTRACTION
 -----------
 Gentoo Linux Next is a specialized remaster of the Gentoo Linux Minimal 
 Installation CD. It preserves the power of Portage while automating the 
 tedious aspects of deployment via a custom Python wrapper. 

 The build process is automated using GitHub Actions, compiling the upstream 
 kernel from source and injecting a custom overlay filesystem into the ISO.

 We do not bundle unnecessary binaries. We build them.

 THE TOOLSET (PKG)
 -----------------
 The system ships with 'pkg', a Python-based wrapper around Portage. 
 Location: /usr/local/bin/pkg

 USAGE:
    # pkg search <query> 
      :: Query the Portage tree (uses local JSON cache for speed)

    # pkg install <atom> [options]
      :: Emerge package with safety checks.
      :: Options:
         -j <n>       : Set make jobs (override config)
         --use "..."  : Apply temporary USE flags
         --dry-run    : Pretend mode

      [EXAMPLE] pkg install app-editors/neovim --use="lua python"

    # pkg remove <atom>
      :: Unmerge package (depclean implied)

    # pkg update
      :: Sync repositories and rebuild the internal JSON cache

    # pkg upgrade
      :: Update @world (Deep update)

 FILESYSTEM HIERARCHY
 --------------------
 The repository structure mirrors the build process.

 1. CONFIGURATION
    config/kernel/config     :: The Linux Kernel .config file.
            
 2. OVERLAY (AiRootFS)
    The 'overlay' directory acts as the filesystem root. Any file placed
    inside this directory is copied over the base Gentoo system during
    the build process.
    
    Use this to inject configuration files (/etc), binaries (/usr/bin),
    or firmware (/lib/firmware) without modifying the build script.

 3. WORKFLOWS
    .github/workflows :: The Build Forge (CI/CD).

 KERNEL COMPILATION
 ------------------
 We do not use the generic Genkernel. We compile upstream sources.
 
 The build workflow fetches the latest stable kernel from kernel.org, 
 applies the custom configuration, and compiles it using the available 
 cores on the host runner.

 ! WARNING ON CONFIGURATION !
 GitHub Actions runners are limited. 
 - Do NOT use 'make allyesconfig'.
 - Disable unused file systems (XFS/BTRFS if not needed).
 - Compile drivers as Modules (M) where possible.

 DEPLOYMENT INSTRUCTIONS
 -----------------------
 1. Modify 'config/kernel/config' to suit your hardware.
 2. Push changes to the 'main' branch.
 3. Navigate to the "Actions" tab.
 4. Select "Build Gentoo Linux Next".
 5. Dispatch the workflow.
 
 Artifacts are generated in ISO format, bootable via Hybrid MBR/EFI.
 Bootloader is GRUB 2.

 COMPATIBILITY
 -------------
 Base System: Gentoo Linux (amd64/multilib)
 Init System: OpenRC (Hardened)
 Bootloader:  GRUB (Hybrid ISO9660)

 DISCLAIMER
 ----------
 THIS SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
 IF YOU BREAK YOUR MBR, THAT IS YOUR PROBLEM.
 IF YOU COMPILE A KERNEL WITHOUT DRIVERS, THAT IS YOUR PROBLEM.
 
 KEEP IT SIMPLE. KEEP IT FAST.
