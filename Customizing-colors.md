The default colors can be overridden by adding entries to `$HOME/.sup/colors.yaml`. This file does not exist and must be created by the user. The default settings would look like this:
```yaml
---
:status:
  :fg: white
  :bg: blue
  :attrs:
  - bold
:index_old:
  :fg: white
  :bg: default
:index_new:
  :fg: white
  :bg: default
  :attrs:
  - bold
:index_starred:
  :fg: yellow
  :bg: default
  :attrs:
  - bold
:index_draft:
  :fg: red
  :bg: default
  :attrs:
  - bold
:labellist_old:
  :fg: white
  :bg: default
:labellist_new:
  :fg: white
  :bg: default
  :attrs:
  - bold
:twiddle:
  :fg: blue
  :bg: default
:label:
  :fg: yellow
  :bg: default
:message_patina:
  :fg: black
  :bg: green
:alternate_patina:
  :fg: black
  :bg: blue
:missing_message:
  :fg: black
  :bg: red
:attachment:
  :fg: cyan
  :bg: default
:cryptosig_valid:
  :fg: yellow
  :bg: default
  :attrs:
  - bold
:cryptosig_valid_untrusted:
  :fg: yellow
  :bg: blue
  :attrs:
  - bold
:cryptosig_unknown:
  :fg: cyan
  :bg: default
:cryptosig_invalid:
  :fg: yellow
  :bg: red
  :attrs:
  - bold
:generic_notice_patina:
  :fg: cyan
  :bg: default
:quote_patina:
  :fg: yellow
  :bg: default
:sig_patina:
  :fg: yellow
  :bg: default
:quote:
  :fg: yellow
  :bg: default
:sig:
  :fg: yellow
  :bg: default
:to_me:
  :fg: green
  :bg: default
:starred:
  :fg: yellow
  :bg: default
  :attrs:
  - bold
:starred_patina:
  :fg: yellow
  :bg: green
  :attrs:
  - bold
:alternate_starred_patina:
  :fg: yellow
  :bg: blue
  :attrs:
  - bold
:snippet:
  :fg: cyan
  :bg: default
:option:
  :fg: white
  :bg: default
:tagged:
  :fg: yellow
  :bg: default
  :attrs:
  - bold
:draft_notification:
  :fg: red
  :bg: default
  :attrs:
  - bold
:completion_character:
  :fg: white
  :bg: default
  :attrs:
  - bold
:horizontal_selector_selected:
  :fg: yellow
  :bg: default
  :attrs:
  - bold
:horizontal_selector_unselected:
  :fg: cyan
  :bg: default
:search_highlight:
  :fg: black
  :bg: yellow
  :attrs:
  - bold
:system_buf:
  :fg: blue
  :bg: default
:regular_buf:
  :fg: white
  :bg: default
:modified_buffer:
  :fg: yellow
  :bg: default
  :attrs:
  - bold
:date:
  :fg: white
  :bg: default
:size_widget:
  :fg: white
  :bg: default
```
As stated previously, only overridden values need to be added to `colors.yaml`, not all of them.

Note: the above file content can be generated like this:
`ruby -I lib -r sup/colormap.rb -r yaml -e 'puts Redwood::Colormap::DEFAULT_COLORS.to_yaml'`