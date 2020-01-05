## Game Loop

A Rust crate that implements a frame-rate-independent game loop. The code is
based on ["Fix Your Timestep!"](https://gafferongames.com/post/fix_your_timestep/),
it's extremely lightweight and supports both native execution and compilation to
wasm.

## Usage

```rust
use game_loop::game_loop;

fn main() {
  let game = YourGame::new();

  game_loop(game, 240, |g| {
    g.game.your_update_function();
  }, |g| {
    g.game.your_render_function();
  });
}
```

The value `240` is the number of updates per second. It is _not_ the frame rate.
In web environments, the frame rate is controlled by
[requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame),
otherwise render is called as quickly as possible, though you can slow it down
with [`std::thread::sleep`](https://doc.rust-lang.org/std/thread/fn.sleep.html)
if you wish. This may be useful if vsync is enabled or to save power on mobile
devices.

The `g` closure argument lets you access your `game` state which can be anything
you like. You can also access the game loop's running time, how many updates
there have been, etc. It also provides a `blending_factor` that you may use in
your render function to interpolate frames and produce smoother animations. See
the article above for more explanation.

In web environments, `game_loop` is asynchronous and returns immediately,
otherwise it blocks until `g.exit()` is called. Other than that, the interface
is exactly the same.

## Example

There's a [Game of Life example](./examples/game_of_life.rs) that shows how to
use the crate. You can run it with:

```sh
cargo run --example game_of_life
```

![Game of Life](./examples/game_of_life.gif)

## License

MIT
