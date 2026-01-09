
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

The only Systemdboot protects weirdly enough is Unified Kernel images in Secure boot, that include a cmdline already. 
So there are still plenty of things vulnerable where you DO not have an option under systemd to protect yourself. Also if you have an entry, that does not have such an option that could be abused to chainload something else.
