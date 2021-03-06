# Color separations and the SVD

This demonstration shows how to work with color channels and build a
simple GUI to explore image compression using the Singular Value
Decomposition (SVD).

```julia
using Images, TestImages, Interact

img = testimage("mandrill")

channels = float(channelview(img))

function rank_approx(M, k)
    U, S, V = svd(M)
    M = U[:, 1:k] * Diagonal(S[1:k]) * V[:, 1:k]'
    M = min.(max.(M, 0.0), 1.)
end

n = 100
@manipulate for k1 in 1:n, k2 in 1:n, k3 in 1:n
    colorview(RGB,
              rank_approx(channels[1,:,:], k1),
              rank_approx(channels[2,:,:], k2),
              rank_approx(channels[3,:,:], k3)
              )
end
```

Here's the result in IJulia:

![mandrill](../assets/demos/color_separations_svd.jpg)

You can click on the slider bars to change the number of components
used in each color channel.
