The [man page](https://www.man7.org/linux/man-pages/man7/systemd-boot.7.html)
A boot loader from Systemd the src can be found [here](https://github.com/systemd/systemd/tree/main/src/boot). 
# Editing of Entries 

the current (as of 5366dbdbd44ba4dc0c914dd4daa1a5297e0b2bde) code that controlls the editing process
```c
case KEYPRESS(0, 0, 'E'):
                        /* only the options of configured entries can be edited */
                        if (!config->editor ||
                            !LOADER_TYPE_ALLOW_EDITOR(config->entries[idx_highlight]->type)) {
                                status = xstrdup16(u"Entry does not support editing the command line.");
                                break;
                        }

                        /* Unified kernels that are signed as a whole will not accept command line options
                         * when secure boot is enabled unless there is none embedded in the image. Do not try
                         * to pretend we can edit it to only have it be ignored. */
                        if (!LOADER_TYPE_ALLOW_EDITOR_IN_SB(config->entries[idx_highlight]->type) &&
                            secure_boot_enabled() &&
                            config->entries[idx_highlight]->options) {
                                status = xstrdup16(u"Entry not editable in SecureBoot mode.");
                                break;
                        }

                        /* The edit line may end up on the last line of the screen. And even though we're
                         * not telling the firmware to advance the line, it still does in this one case,
                         * causing a scroll to happen that screws with our beautiful boot loader output.
                         * Since we cannot paint the last character of the edit line, we simply start
                         * at x-offset 1 for symmetry. */
                        print_at(1, y_status, COLOR_EDIT, clearline + 2);
                        if (line_edit(&config->entries[idx_highlight]->options, x_max - 2, y_status))
                                action = ACTION_RUN;
                        print_at(1, y_status, COLOR_NORMAL, clearline + 2);

                        /* The options string was now edited, hence we have to pass it to the invoked
                         * binary. */
                        config->entries[idx_highlight]->options_implied = false;
                        break;
```


Systemdboot seemingly has an option to toggle to prevent editing. 
as it is apparent in https://old.reddit.com/r/archlinux/comments/x2xvd8/cant_edit_systemdboot_entries_with_e_key/

the file is loaced at `/boot/loader/loader.conf ` :

```loader.conf
editor no
```
disables the editing for all entries and is what should be chosen if you have secure boot. 
This is a footnote in the [man page](https://www.man7.org/linux/man-pages/man5/loader.conf.5.html) this should also be included in some other sources. It is currently not.

```c
 typedef struct {
          BootEntry **entries;
          size_t n_entries;
          size_t idx_default;
          size_t idx_default_efivar;
          uint64_t timeout_sec; /* Actual timeout used (efi_main() override > smbios > efiva     r > config). */
          uint64_t timeout_sec_smbios;
          uint64_t timeout_sec_config;
          uint64_t timeout_sec_efivar;
          char16_t *entry_default_config;
          char16_t *entry_default_efivar;
          char16_t *entry_oneshot;
          char16_t *entry_saved;
          char16_t *entry_sysfail;
          bool editor;
          bool auto_entries;
          bool auto_firmware;
          bool auto_poweroff;
          bool auto_reboot;
          bool reboot_for_bitlocker;
          RebootOnError reboot_on_error;
          secure_boot_enroll secure_boot_enroll;
          secure_boot_enroll_action secure_boot_enroll_action;
          uint64_t secure_boot_enroll_timeout_sec;
          bool force_menu;
          bool use_saved_entry;
          bool use_saved_entry_efivar;
          bool beep;
          bool sysfail_occured;
          int64_t console_mode;
          int64_t console_mode_efivar;
  } Config;
```