# Millennium Cyborg's StreamFX Shaders

##  Putting Together the Reflection Effect

- Your avatar needs to trigger the conditions for the bug I'm exploiting,
  I recommend:
  - VRM avatar in VSeeFace
  - Glasses material using 'Standard' shader in 'Transparent' rendering mode
    - Colour: black with alpha between 0.5 and 0.95

- Scene 'Avatar Lighting'
  - 'Avatar' - avatar source
  - Source for other accessories that should receive lighting
  - 'Game Mirrored Blurred' - source mirror of game source (hidden)
    - Blur: Dual Filtering, Area, Size 6.0
    - 3D Transform: Orthographic, Scale X -100%
  - (On Scene) Dynamic Mask
    - Input: 'Game Mirrored Blurred'
    - Red Channel: Base 0.2, Red Input 1.0
    - Green Channel: Base 0.2, Green Input 1.0
    - Blue Channel: Base 0.2, Blue Input 1.0
- Scene 'Avatar Reflection':
  - 'Reflection Mask' - source mirror of avatar source (hidden)
    - Shader: 01-alpha-to-mask-and-erode.effect
      - Thresholds can be used to help get rid of stray pixels, make sure you keep glasses surface within threshold
    - Blur: Dual Filtering, Area, Size 1.0
  - 'Blend Group' - Group
    - 'Reflection' - source mirror of game source (hidden)
      - Shader: 02-sphere-distort.effect
        - Default values should be a reasonable starting point
        - Try to make a sphere behind your head that follows glasses surface as you move your head around
        - Play with the "Tint" option at the bottom
      - Blue: Box Linear, Area, Size 3.0
      - Dynamic Mask
        - Input: 'Reflection Mask'
        - Alpha Channel: Base 0.0, Red Input 2.5 (this will bias/grow the mask after the blur on it)
    - 'Avatar Lighting' - Nested scene from last step
      - Edit Transform: Y Position = 1080 (or whatever your canvas height is)
    - (On Group) Shader: 03-blend-group-special.effect
      - Special Sauce: Enabled
      - Upper Source Gain: Control amount of reflection
      - Lower Source Gain: Control amount of transmission

Now drop scene 'Avatar Reflection' in where you want your avatar!