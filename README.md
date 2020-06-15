# RustValora
Testing code in Rust with Valora to create shapes and animations

# How do I get started?
Read the documentation for the Valora project here: https://paytonturnage.gitbook.io/valora/ and https://github.com/turnage/valora

# What can you do?
You can use Valora to create generative art.

Some sample code:

# Moving Circles inside of repeating triangles with inverse colours
Inside your main.rs:
use valora::prelude::*;

fn main() -> Result<()> {
    run_fn(Options::from_args(), |_gpu, world, _rng| {
        Ok(move |ctx: Context, canvas: &mut Canvas| {
            canvas.set_color(LinSrgb::new(1., 1., 1.));
            canvas.paint(Filled(ctx.world));

            //Circle 1 (blue)
            let max_radius = world.width / 5.;
            let radius = ctx.time.as_secs_f32().cos().abs() * max_radius;

            canvas.set_color(LinSrgb::new(0., 0., 1.));
            canvas.paint(Filled(Ellipse::circle(world.center(), radius)));
            
            //Circle 2 (black)
            let max_radius = world.width / 22.;
            let radius = ctx.time.as_secs_f32().cos().abs() * max_radius;

            canvas.set_color(LinSrgb::new(0., 0., 0.));
            canvas.paint(Filled(Ellipse::circle(world.center(), radius)));
            
            //Triangle

            canvas.set_color(LinSrgb::new(1., 0., 0.));
            let triangle = Ngon::triangle(world.center(), 200.);
            let repeated = std::iter::successors(Some(triangle), |t| {
             Some(t.clone().scale(0.9))
            });
            
            //Repeat triangles 15 times
            for triangle in repeated.take(15) {
                canvas.paint(Stroked {
                    width: 3.,
                    element: triangle,
                });
            }
        })
    })
}
