# 软件开发

## Hexo

- [hexo - blog framework](https://hexo.io/index.html)
- [hexo theme](https://www.theme-next.org/index.html)

## Markdown

- [markdown 指南](https://www.markdownguide.org/basic-syntax/)
- [github 关于 markdown 的文档](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## 媒体

- [X(twitter)](https://developer.twitter.com/en)
- [Tweepy](https://www.tweepy.org/)
- [今日热榜](https://tophub.today/)

## API

- [OpenWeatherMap](https://openweathermap.org)

## Artificial Intelligence

- [openai cookbook](https://cookbook.openai.com/examples/how_to_format_inputs_to_chatgpt_models)

## Remote Desktop RDP & VNC

### From iPad remote access Ubuntu

- [Use iPad as second monitor ubuntu](https://www.omgubuntu.co.uk/2022/06/use-ipad-as-second-monitor-ubuntu-22-04)

  ```bash
  gsettings set org.gnome.desktop.remote-desktop.rdp screen-share-mode extend
  ```

- <https://www.itpro.com/mobile/remote-access/368102/how-to-remote-desktop-into-ubuntu>
- <https://linustechtips.com/topic/1402047-remote-desktop-from-ipad-to-linux-pc/>
- <https://www.nomachine.com/getting-started-with-nomachine-for-ios>

- <https://www.realvnc.com/en/>

  For realvnc you'll need to sign in and get a license to run.

- Remote Ripple works but very slow.

## Free resources

- [free for dev](https://free-for.dev)
  - [repo](https://github.com/ripienaar/free-for-dev)

## Makefile autocompletion on Mac

[Makefile autocompletion on Mac](https://stackoverflow.com/questions/33760647/makefile-autocompletion-on-mac)

> This worked for me on Catalina in the standard zsh terminal

Edit the file named .zshrc in you home directory this can be done with this command in the terminal nano ~/.zshrc
Add the following lines

```sh
zstyle ':completion:*:*:make:*' tag-order 'targets'

autoload -U compinit && compinit
```

## Animation

- [OpenToonz](https://opentoonz.github.io/e/)
