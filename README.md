# perception-aware-dynamic-diffusion-transformers

# PA-D²iT — Perception-Aware Dynamic Diffusion Transformer

**Put the model's generative capacity where the driving risk is.**

> Entropy tells the model where the pixels are busy.
> It never tells it where the pedestrian is.

Dynamic diffusion transformers like D²iT allocate fine- vs. coarse-grained
capacity from a Shannon-entropy map — which is really a texture detector. In a
driving scene that sends the model's finest capacity to the textured tree line
and leaves the dark-clothed pedestrian coarse. But a few hundred pixels on a
vulnerable road user are exactly the pixels that decide whether the car brakes.

**PA-D²iT swaps that one signal.** Instead of deriving the coarse/fine grain map
from image entropy, we derive it from a *layout-driven importance field* —
vulnerable road users get the highest importance, vehicles less, static
background least — so fine-grained tokens land on the objects that actually
matter. Because the grain map comes from the layout rather than the noisy
latent, it's deterministic and identical at every timestep, which sidesteps the
noisy-routing failure that plagues diffusion-MoE routers at high `t`.

Everything downstream — the dynamic transformer, the multi-grained loss — is
inherited unchanged from D²iT. Same machinery, different signal. That's what
makes the hypothesis cleanly testable **at matched compute**.

We evaluate on the metrics that matter — object-region FID and downstream
detector mAP on safety-critical crops — with global FID as a guardrail against
wrecking the background.

**One block changes. We measure whether it was worth it.**
