\# 🧱 OpenBrickVR — Open-Source VR Brick Builder \& Step-by-Step Set Simulator



\[!\[Status: RFC / Architecture Phase](https://img.shields.io/badge/Status-RFC\_%2F\_Architecture\_Phase-amber)](#)

\[!\[License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](#)

\[!\[OpenXR](https://img.shields.io/badge/VR-OpenXR-green)](#)

\[!\[LDraw Compatible](https://img.shields.io/badge/Assets-LDraw\_Standard-red)](#)



> \*\*An open-source successor to BrickVR.\*\* Build official sets step-by-step from community `.ldr` / `.mpd` / `.io` files or free-build in virtual reality with deterministic, rule-based brick snapping powered by the LDraw \& LDCad Shadow Library ecosystem.



\---



\## 🎯 The Vision \& Problem Statement



Five years ago, \*\*BrickVR\*\* proved how magical building with interlocking plastic bricks in VR could be—following instruction manuals step-by-step and snapping parts together in 3D space. However, BrickVR was closed-source, has been abandoned for years, and lacked modern OpenXR hand-tracking, custom set importing, and performance optimizations for massive builds (5,000+ pieces).



Meanwhile, the open-source brick community has already solved the hardest data problems on flat screens:

1\. \*\*The LDraw Parts Library\*\* contains 3D geometry for almost every brick ever produced.

2\. \*\*The LDCad Shadow Library\*\* (`!LDCAD SNAP\_\*` meta-commands) already defines exact mathematical connection points (studs, anti-studs, Technic pin holes, axles, clips, and bars) for thousands of parts.

3\. \*\*OMR (Official Model Repository) \& BrickLink Studio (`.io`)\*\* provide thousands of official and custom sets pre-formatted with `0 STEP` instruction markers.



\*\*OpenBrickVR\*\* bridges these existing open ecosystems into a modern, high-performance VR engine—designed from the ground up so developers using AI coding workflows (Claude Code, Cursor, Windsurf) can easily build and test isolated modules.



\---



\## 🏗️ System Architecture



```text

+-----------------------------------------------------------------------+

|                        Input \& Asset Layer                            |

|  \[LDraw Library (.dat)] + \[LDCad Shadow (.dat)] + \[Model (.mpd/.io)]  |

+-----------------------------------+-----------------------------------+

&#x20;                                   |

&#x20;                                   v

+-----------------------------------------------------------------------+

|                       Core Parser \& Cache Pipeline                    |

|  1. LDraw Geometry Tessellator (BFC winding, sub-part instancing)     |

|  2. Snap Metadata Extractor (!LDCAD SNAP\_CYL, SNAP\_CLP, SNAP\_FGR)     |

|  3. Instruction Step Sequencer (0 STEP / 0 ROTSTEP state machine)     |

+-----------------------------------+-----------------------------------+

&#x20;                                   |

&#x20;                 +-----------------+-----------------+

&#x20;                 |                                   |

&#x20;                 v                                   v

+-----------------------------------+   +-------------------------------+

|    Guided Instruction Mode        |   |      Free-Build Engine        |

| - Spawns parts per active step    |   | - Spatial Octree snap queries |

| - Renders translucent target mesh |   | - Graph-based sub-assemblies  |

| - Distance + Quaternion validation|   | - OBB collision validation    |

+-----------------+-----------------+   +---------------+---------------+

&#x20;                 |                                   |

&#x20;                 +-----------------+-----------------+

&#x20;                                   |

&#x20;                                   v

+-----------------------------------------------------------------------+

|                    VR Interaction \& Rendering Layer                   |

| - OpenXR Controllers \& Hand Pinch   - Dynamic Mesh Combining / GPU    |

| - Magnetic Raycast \& Micro-Align      Instancing (Single Draw Call)   |

+-----------------------------------------------------------------------+

