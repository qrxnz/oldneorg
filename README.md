<div align="center">

  <img src="res/neorg.svg" width=300>

# Neorg - An Organized Future

</div>



## 🔧 Installation/Quickstart

**Neorg requires at least Neovim 0.8+ to operate.**

  <details>
  <summary>sample init.lua</summary>

  ```lua
  -- adapted from https://github.com/folke/lazy.nvim#-installation

  local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
  if not vim.loop.fs_stat(lazypath) then
    print(vim.fn.system({
      "git",
      "clone",
      "--filter=blob:none",
      "https://github.com/folke/lazy.nvim.git",
      "--branch=stable",
      lazypath,
    }))
  end
  vim.opt.rtp:prepend(lazypath)
  
  vim.g.mapleader = " "
  
  require("lazy").setup({
    "rebelot/kanagawa.nvim",  -- neorg needs a colorscheme with treesitter support
    {
      "nvim-treesitter/nvim-treesitter",
      build = ":TSUpdate",
      opts = {
        highlight = { enable = true },
      },
      config = function(_, opts)
        require("nvim-treesitter.configs").setup(opts)
      end,
    },
    {
      "nvim-neorg/neorg",
      build = ":Neorg sync-parsers",
      dependencies = { "nvim-lua/plenary.nvim" },
      config = function()
        require("neorg").setup {
          load = {
            ["core.defaults"] = {},
            ["core.concealer"] = {},
            ["core.dirman"] = {
              config = {
                workspaces = {
                  notes = "~/notes",
                },
                default_workspace = "notes",
              },
            },
          },
        }
  
        vim.wo.foldlevel = 99
        vim.wo.conceallevel = 2
      end,
    }
  })
  
  vim.cmd.colorscheme('kanagawa')
  ```

  </details>

### Installation

You can install it through your favorite plugin manager:

-
  <details>
  <summary><a href="https://github.com/wbthomason/packer.nvim">packer.nvim</a></summary>

  ```lua
  use {
      "qrxnz/oldneorg",
      config = function()
          require('neorg').setup {
              load = {
                  ["core.defaults"] = {}, -- Loads default behaviour
                  ["core.concealer"] = {}, -- Adds pretty icons to your documents
                  ["core.dirman"] = { -- Manages Neorg workspaces
                      config = {
                          workspaces = {
                              notes = "~/notes",
                          },
                      },
                  },
              },
          }
      end,
      run = ":Neorg sync-parsers",
      requires = "nvim-lua/plenary.nvim",
  }
  ```

  Every time Neorg hits a new release, a new tag is created by us, so you don't have to worry about all the updates inbetween.
  That means that adding `tag = "*"` in Packer will update to latest stable release.

  You can also pin Neorg to one specific version through e.g. `tag = "2.0.0"`.

  ---

  Want to lazy load? You can use the `ft` key to load Neorg only upon entering a `.norg` file:

  ```lua
  use {
      "qrxnz/oldneorg",
      -- tag = "*",
      ft = "norg",
      after = "nvim-treesitter", -- You may want to specify Telescope here as well
      config = function()
          require('neorg').setup {
              load = {
                  ["core.defaults"] = {}, -- Loads default behaviour
                  ["core.concealer"] = {}, -- Adds pretty icons to your documents
                  ["core.dirman"] = { -- Manages Neorg workspaces
                      config = {
                          workspaces = {
                              notes = "~/notes",
                          },
                      },
                  },
              },
          }
      end
  }
  ```

  Although it's proven to work for a lot of people, you might need to take some
  additional steps depending on how your lazyloading system and/or Neovim
  config is set up.

  </details>

- <details>
  <summary><a href="https://github.com/junegunn/vim-plug">vim-plug</a></summary>

  ```vim
  Plug 'qrxnz/oldneorg' | Plug 'nvim-lua/plenary.nvim'
  ```

  You can then put this initial configuration in your `init.vim` file:

  ```vim
  lua << EOF
  require('neorg').setup {
      load = {
          ["core.defaults"] = {}, -- Loads default behaviour
          ["core.concealer"] = {}, -- Adds pretty icons to your documents
          ["core.dirman"] = { -- Manages Neorg workspaces
              config = {
                  workspaces = {
                      notes = "~/notes",
                  },
              },
          },
      },
  }
  EOF
  ```

  </details>
- <details>
  <summary><a href="https://github.com/folke/lazy.nvim">lazy.nvim</a></summary>

  ```lua
  require("lazy").setup({
    {
      "qrxnz/oldneorg",
      build = ":Neorg sync-parsers",
      dependencies = { "nvim-lua/plenary.nvim" },
      config = function()
        require("neorg").setup {
          load = {
            ["core.defaults"] = {}, -- Loads default behaviour
            ["core.concealer"] = {}, -- Adds pretty icons to your documents
            ["core.dirman"] = { -- Manages Neorg workspaces
              config = {
                workspaces = {
                  notes = "~/notes",
                },
              },
            },
          },
        }
      end,
    },
  })
  ```

  </details>


With the above `run` key set, every time you update Neorg the internal parsers
will also be updated to the correct revision.
