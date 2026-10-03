# iced_winit (fork com suporte a Android)

Fork do [`iced_winit`](https://crates.io/crates/iced_winit) **0.14.1** (MIT,
© Héctor Ramón Jiménez) usado pelo preset `android` do
[glacier-ui](https://github.com/antoniofernandodj/glacier-ui). O pacote mantém o
nome `iced_winit` de propósito: o `[patch.crates-io]` só substitui um crate por
outro de mesmo nome.

```toml
[patch.crates-io]
iced_winit = { git = "https://github.com/antoniofernandodj/iced_winit", tag = "0.14.1-android" }
```

## O que muda

Duas mudanças, ambas atrás de `#[cfg(target_os = "android")]` — em qualquer outro
alvo o crate é idêntico ao publicado (diff completo em `ANDROID.patch`):

1. `src/lib.rs` — o `winit` 0.30 só cria o `EventLoop` no Android com o
   `AndroidApp` do `android_main`. O módulo `iced_winit::android` guarda esse
   valor (`set_android_app`) e o `run()` o entrega ao
   `EventLoopBuilderExtAndroid::with_android_app`.
2. `src/conversion.rs` — `winit::platform::modifier_supplement` não existe no
   Android; lá o teclado usa `logical_key`/`text` direto.

Ao subir o `iced`, refazer a partir do `iced_winit` novo e reaplicar o patch
(`patch -p2 < ANDROID.patch` dentro de uma cópia da versão original).
