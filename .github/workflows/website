<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Minecraft — Voxel Sandbox</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { height: 100%; overflow: hidden; background: #000; font-family: 'Segoe UI', system-ui, sans-serif; user-select: none; -webkit-user-select: none; }
  canvas { display: block; }

  /* Menu System Styles */
  #menu-container { position: fixed; inset: 0; z-index: 100; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, #1a1410 0%, #3a2a1c 50%, #1a1410 100%); overflow: hidden; }
  #menu-container::before { content: ''; position: absolute; inset: -10%; background: radial-gradient(ellipse at 20% 30%, rgba(180,130,70,0.25) 0%, transparent 60%), radial-gradient(ellipse at 80% 70%, rgba(100,70,30,0.3) 0%, transparent 60%); pointer-events: none; animation: drift 20s ease-in-out infinite alternate; }
  @keyframes drift { 0% { transform: translate(0,0); } 100% { transform: translate(-3%,2%); } }

  .menu-screen { display: none; flex-direction: column; align-items: center; color: #ddd; z-index: 2; position: relative; width: 400px; max-width: 90vw; }
  .menu-screen.active { display: flex; }

  .menu-title { font-size: clamp(48px, 9vw, 96px); font-weight: 900; letter-spacing: -3px; background: linear-gradient(180deg, #e8c890 0%, #b88848 60%, #6a4828 100%); -webkit-background-clip: text; background-clip: text; color: transparent; text-shadow: 0 6px 30px rgba(232,200,144,0.3); line-height: 0.9; transform: perspective(500px) rotateX(12deg); margin-bottom: 30px; }
  .menu-subtitle { font-size: 14px; color: #8a7050; letter-spacing: 8px; text-transform: uppercase; font-weight: 600; margin-bottom: 40px; }
  .menu-header { font-size: 24px; font-weight: 700; color: #e8c890; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 30px; }

  .menu-btn { width: 100%; background: linear-gradient(180deg, #7cb342 0%, #558b2f 100%); border: 3px solid #1a1410; color: #fff; padding: 14px 20px; font-size: 18px; font-weight: 800; cursor: pointer; text-shadow: 2px 2px 0 rgba(0,0,0,0.4); box-shadow: 0 5px 0 #1a1410, 0 8px 20px rgba(0,0,0,0.5), inset 0 2px 0 rgba(255,255,255,0.3); transition: transform 0.1s, box-shadow 0.1s; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 15px; }
  .menu-btn:hover { transform: translateY(-2px); box-shadow: 0 7px 0 #1a1410, 0 12px 25px rgba(0,0,0,0.6), inset 0 2px 0 rgba(255,255,255,0.4); }
  .menu-btn:active { transform: translateY(3px); box-shadow: 0 2px 0 #1a1410, 0 4px 10px rgba(0,0,0,0.4); }
  .menu-btn.secondary { background: linear-gradient(180deg, #8a6a3a 0%, #5a4528 100%); }
  .menu-btn.danger { background: linear-gradient(180deg, #c0392b 0%, #922b21 100%); }
  .menu-btn.toggle { width: 80px; padding: 8px; margin: 0; font-size: 14px; }
  .menu-btn.toggle.on { background: linear-gradient(180deg, #27ae60 0%, #16a085 100%); }
  .menu-btn.toggle.off { background: linear-gradient(180deg, #7f8c8d 0%, #5a4528 100%); }

  .menu-input { width: 100%; background: #1a1410; border: 2px solid #4a3520; color: #e8c890; padding: 12px; font-size: 16px; margin-bottom: 20px; font-family: 'Consolas', monospace; outline: none; }
  .menu-input:focus { border-color: #e8c890; }

  .settings-row { width: 100%; margin-bottom: 20px; }
  .settings-label { display: flex; justify-content: space-between; align-items: center; color: #aa9c80; font-size: 14px; margin-bottom: 8px; text-transform: uppercase; letter-spacing: 1px; }
  .settings-label span { color: #e8c890; font-weight: bold; }
  .settings-slider { -webkit-appearance: none; width: 100%; height: 8px; background: #1a1410; border: 1px solid #4a3520; outline: none; }
  .settings-slider::-webkit-slider-thumb { -webkit-appearance: none; appearance: none; width: 20px; height: 20px; background: #e8c890; cursor: pointer; border: 2px solid #1a1410; }
  .settings-slider::-moz-range-thumb { width: 20px; height: 20px; background: #e8c890; cursor: pointer; border: 2px solid #1a1410; }

  /* Loading Bar Styles */
  .loading-bar-container { width: 100%; height: 24px; background: #1a1410; border: 2px solid #4a3520; margin-bottom: 20px; position: relative; }
  .loading-bar-fill { width: 0%; height: 100%; background: linear-gradient(90deg, #558b2f 0%, #7cb342 100%); transition: width 0.1s; }
  .loading-text { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #fff; font-family: 'Consolas', monospace; font-size: 12px; font-weight: bold; text-shadow: 1px 1px 0 #000; }

  /* HUD Styles */
  #crosshair { position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%); pointer-events: none; width: 22px; height: 22px; z-index: 10; mix-blend-mode: difference; display: none; }
  #crosshair::before, #crosshair::after { content: ''; position: absolute; background: #fff; }
  #crosshair::before { top: 50%; left: 0; right: 0; height: 2px; transform: translateY(-50%); }
  #crosshair::after { left: 50%; top: 0; bottom: 0; width: 2px; transform: translateX(-50%); }

  #hotbar { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%); display: none; gap: 4px; padding: 6px; background: rgba(20,14,10,0.7); border: 2px solid rgba(255,255,255,0.15); z-index: 10; }
  .slot { width: 54px; height: 54px; background: rgba(80,80,80,0.3); border: 2px solid rgba(40,40,40,0.6); position: relative; display: flex; align-items: center; justify-content: center; transition: border-color 0.15s, background 0.15s, transform 0.15s; }
  .slot.selected { border-color: #fff; background: rgba(255,255,255,0.18); box-shadow: 0 0 0 1px rgba(255,255,255,0.6), 0 0 16px rgba(255,255,255,0.25); transform: translateY(-4px); }
  .slot canvas { width: 42px; height: 42px; image-rendering: pixelated; pointer-events: none; }
  .slot-num { position: absolute; top: 2px; left: 4px; font-size: 10px; color: #fff; text-shadow: 1px 1px 0 #000; font-family: 'Consolas', monospace; font-weight: 700; pointer-events: none; }

  #stats { position: fixed; top: 14px; left: 14px; color: #fff; font-family: 'Consolas', monospace; font-size: 13px; text-shadow: 1px 1px 0 #000; z-index: 10; line-height: 1.7; pointer-events: none; display: none; }
  #stats .label { color: #ffcb6b; }

  #toast { position: fixed; top: 50%; left: 50%; transform: translate(-50%,-150px); background: rgba(0,0,0,0.75); color: #fff; padding: 10px 22px; font-size: 14px; font-weight: 600; letter-spacing: 1px; opacity: 0; transition: opacity 0.25s; pointer-events: none; z-index: 11; border: 1px solid rgba(255,255,255,0.2); }
  #toast.show { opacity: 1; }

  #water-overlay { position: fixed; inset: 0; pointer-events: none; z-index: 5; background: rgba(40,100,180,0.28); display: none; }
  #water-overlay.show { display: block; }

  #inventory { position: fixed; inset: 0; background: rgba(0,0,0,0.6); display: none; align-items: center; justify-content: center; z-index: 50; backdrop-filter: blur(4px); }
  #inventory.show { display: flex; }
  .inv-window { background: linear-gradient(180deg, #2a1f15 0%, #1a1410 100%); border: 3px solid #4a3520; box-shadow: 0 10px 40px rgba(0,0,0,0.8); padding: 20px; width: 600px; max-width: 90vw; color: #ddd; }
  .inv-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; border-bottom: 1px solid #4a3520; padding-bottom: 10px; }
  .inv-title { font-size: 18px; font-weight: 700; color: #e8c890; letter-spacing: 1px; text-transform: uppercase; }
  .inv-close { background: #aa3333; color: #fff; border: 1px solid #882222; padding: 5px 12px; cursor: pointer; font-weight: 700; font-family: 'Consolas', monospace; }
  .inv-close:hover { background: #cc4444; }
  .inv-body { display: flex; gap: 20px; }
  .inv-tabs { display: flex; flex-direction: column; gap: 5px; width: 120px; }
  .inv-tab { padding: 10px; background: #1a1410; border: 1px solid #4a3520; cursor: pointer; text-align: center; font-size: 13px; font-weight: 600; color: #aa9c80; transition: all 0.2s; }
  .inv-tab:hover { background: #2a1f15; color: #ddd; }
  .inv-tab.active { background: #4a3520; color: #e8c890; border-color: #6a5530; }
  .inv-content { flex: 1; background: #1a1410; border: 1px solid #4a3520; padding: 10px; min-height: 300px; }
  .inv-grid { display: grid; grid-template-columns: repeat(9, 1fr); gap: 6px; }
  .inv-item { width: 100%; aspect-ratio: 1; background: rgba(80,80,80,0.3); border: 2px solid rgba(40,40,40,0.6); display: flex; align-items: center; justify-content: center; cursor: pointer; transition: all 0.15s; }
  .inv-item:hover { border-color: #aaa; background: rgba(120,120,120,0.3); }
  .inv-item.selected { border-color: #e8c890; background: rgba(232,200,144,0.2); box-shadow: 0 0 10px rgba(232,200,144,0.3); }
  .inv-item canvas { width: 80%; height: 80%; image-rendering: pixelated; pointer-events: none; }
  .inv-hotbar-section { margin-top: 20px; border-top: 1px solid #4a3520; padding-top: 15px; }
  .inv-hotbar-label { font-size: 12px; color: #aa9c80; margin-bottom: 8px; text-transform: uppercase; letter-spacing: 1px; }
  .inv-hotbar { display: grid; grid-template-columns: repeat(9, 1fr); gap: 6px; }
  .inv-hb-slot { aspect-ratio: 1; background: rgba(80,80,80,0.3); border: 2px solid rgba(40,40,40,0.6); display: flex; align-items: center; justify-content: center; cursor: pointer; transition: all 0.15s; position: relative; }
  .inv-hb-slot:hover { border-color: #fff; }
  .inv-hb-slot canvas { width: 80%; height: 80%; image-rendering: pixelated; pointer-events: none; }
  .inv-hb-slot.num { position: absolute; top: 2px; left: 4px; font-size: 10px; color: #fff; text-shadow: 1px 1px 0 #000; font-family: 'Consolas', monospace; font-weight: 700; pointer-events: none; }
  .inv-instructions { margin-top: 15px; font-size: 11px; color: #776650; text-align: center; font-style: italic; }
  
  /* Cursor Item */
  #cursor-item { position: fixed; pointer-events: none; display: none; z-index: 100; transform: translate(-50%, -50%); }
  #cursor-item canvas { width: 42px; height: 42px; image-rendering: pixelated; }
</style>
</head>
<body>

<div id="menu-container">
  <div id="screen-main" class="menu-screen active">
    <div class="menu-title">MINECRAFT</div>
    <div class="menu-subtitle">Voxel Sandbox</div>
    <button class="menu-btn" id="btn-new-main">New World</button>
    <button class="menu-btn secondary" id="btn-load-main">Load World</button>
    <button class="menu-btn secondary" id="btn-settings-main">Settings</button>
  </div>

  <div id="screen-new" class="menu-screen">
    <div class="menu-header">Create New World</div>
    <input type="text" class="menu-input" id="seed-input" placeholder="World Seed (Leave empty for random)">
    <button class="menu-btn" id="btn-create-world">Create World</button>
    <button class="menu-btn danger" id="btn-cancel-new">Cancel</button>
  </div>

  <div id="screen-loading" class="menu-screen">
    <div class="menu-header">Generating World...</div>
    <div class="loading-bar-container">
      <div class="loading-bar-fill" id="load-fill"></div>
      <div class="loading-text" id="load-text">0%</div>
    </div>
    <button class="menu-btn" id="btn-enter-game" style="display: none; margin-top: 20px;">Enter World</button>
  </div>

  <div id="screen-settings" class="menu-screen">
    <div class="menu-header">Settings</div>
    <div class="settings-row">
      <div class="settings-label">Mouse Sensitivity <span id="val-sens">1.0</span></div>
      <input type="range" class="settings-slider" id="slider-sens" min="0.1" max="2" step="0.1" value="1.0">
    </div>
    <div class="settings-row">
      <div class="settings-label">Field of View <span id="val-fov">75</span></div>
      <input type="range" class="settings-slider" id="slider-fov" min="60" max="100" step="1" value="75">
    </div>
    <div class="settings-row">
      <div class="settings-label">Render Distance <span id="val-render">72</span></div>
      <input type="range" class="settings-slider" id="slider-render" min="20" max="100" step="1" value="72">
    </div>
    <div class="settings-row" style="display: flex; justify-content: space-between; align-items: center;">
      <div class="settings-label" style="margin-bottom: 0;">Fancy Graphics (Shaders)</div>
      <button class="menu-btn toggle on" id="toggle-fancy">ON</button>
    </div>
    <div class="settings-row" style="display: flex; justify-content: space-between; align-items: center;">
      <div class="settings-label" style="margin-bottom: 0;">See-Through Glass</div>
      <button class="menu-btn toggle on" id="toggle-glass">ON</button>
    </div>
    <div class="settings-row" style="display: flex; justify-content: space-between; align-items: center;">
      <div class="settings-label" style="margin-bottom: 0;">Waving Leaves</div>
      <button class="menu-btn toggle on" id="toggle-leaves">ON</button>
    </div>
    <div class="settings-row" style="display: flex; justify-content: space-between; align-items: center;">
      <div class="settings-label" style="margin-bottom: 0;">Shooting Stars</div>
      <button class="menu-btn toggle on" id="toggle-stars">ON</button>
    </div>
    <button class="menu-btn" id="btn-back-settings">Save & Back</button>
  </div>

  <div id="screen-pause" class="menu-screen">
    <div class="menu-header">Paused</div>
    <button class="menu-btn" id="btn-resume">Resume Game</button>
    <button class="menu-btn secondary" id="btn-settings-pause">Settings</button>
    <button class="menu-btn secondary" id="btn-save-game">Save Game</button>
    <button class="menu-btn danger" id="btn-quit-game">Quit to Title</button>
  </div>
</div>

<div id="crosshair"></div>
<div id="hotbar"></div>
<div id="stats"></div>
<div id="toast"></div>
<div id="water-overlay"></div>

<div id="inventory">
  <div class="inv-window">
    <div class="inv-header">
      <div class="inv-title">Creative Inventory</div>
      <button class="inv-close" id="invCloseBtn">CLOSE [E]</button>
    </div>
    <div class="inv-body">
      <div class="inv-tabs">
        <div class="inv-tab active" data-tab="all">All Blocks</div>
        <div class="inv-tab" data-tab="building">Building</div>
        <div class="inv-tab" data-tab="nature">Nature</div>
        <div class="inv-tab" data-tab="special">Special</div>
        <div class="inv-tab" data-tab="guns">Guns</div>
        <div class="inv-tab" data-tab="player">Player</div>
      </div>
      <div class="inv-content">
        <div class="inv-grid" id="invGrid"></div>
      </div>
    </div>
    <div class="inv-hotbar-section">
      <div class="inv-hotbar-label">Hotbar (Click a block above, then click a slot to swap)</div>
      <div class="inv-hotbar" id="invHotbar"></div>
    </div>
    <div class="inv-instructions">Press E or ESC to close and resume playing</div>
  </div>
</div>
<div id="cursor-item"><canvas width="42" height="42"></canvas></div>

<script type="importmap">
{ "imports": { 
  "three": "https://unpkg.com/three@0.160.0/build/three.module.js", 
  "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/" 
} }
</script>

<script type="module">
import * as THREE from 'three';
import { PointerLockControls } from 'three/addons/controls/PointerLockControls.js';
import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
import { ShaderPass } from 'three/addons/postprocessing/ShaderPass.js';
import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';

// ============================ CONSTANTS ============================
const WORLD_SIZE = 64, WORLD_HEIGHT = 40, CHUNK_SIZE = 16, SEA_LEVEL = 14;
const CHUNKS_PER_SIDE = WORLD_SIZE / CHUNK_SIZE;
const PLAYER_W = 0.6, PLAYER_H = 1.8, PLAYER_EYE = 1.62;
const GRAVITY = 28, JUMP_V = 9, WALK = 4.5, SPRINT = 7.5, REACH = 5;

const AIR=0,GRASS=1,DIRT=2,STONE=3,WOOD=4,LEAVES=5,SAND=6,WATER_SOURCE=7,PLANKS=8,COBBLE=9,GLASS=10,BRICK=11,TNT=12;
const WATER_FLOW_1=13, WATER_FLOW_2=14, WATER_FLOW_3=15;
const PISTOL=20, BAZOOKA=21, TORCH=22, TORCH_N=23, TORCH_S=24, TORCH_E=25, TORCH_W=26;

const TILES = {
  grass_top:[0,0], grass_side:[1,0], dirt:[2,0], stone:[3,0],
  log_top:[0,1], log_side:[1,1], leaves:[2,1], sand:[3,1],
  water:[0,2], planks:[1,2], cobble:[2,2], glass:[3,2], brick:[0,3],
  tnt_top:[1,3], tnt_side:[2,3], tnt_bottom:[3,3],
  torch:[4,0]
};
const ATLAS_N = 5, TILE_PX = 16;

const BLOCK_PROPS = {
  [GRASS]:{top:'grass_top',side:'grass_side',bottom:'dirt'},
  [DIRT]:{top:'dirt',side:'dirt',bottom:'dirt'},
  [STONE]:{top:'stone',side:'stone',bottom:'stone'},
  [WOOD]:{top:'log_top',side:'log_side',bottom:'log_top'},
  [LEAVES]:{top:'leaves',side:'leaves',bottom:'leaves'},
  [SAND]:{top:'sand',side:'sand',bottom:'sand'},
  [WATER_SOURCE]:{top:'water',side:'water',bottom:'water',transparent:true,liquid:true},
  [WATER_FLOW_1]:{top:'water',side:'water',bottom:'water',transparent:true,liquid:true},
  [WATER_FLOW_2]:{top:'water',side:'water',bottom:'water',transparent:true,liquid:true},
  [WATER_FLOW_3]:{top:'water',side:'water',bottom:'water',transparent:true,liquid:true},
  [PLANKS]:{top:'planks',side:'planks',bottom:'planks'},
  [COBBLE]:{top:'cobble',side:'cobble',bottom:'cobble'},
  [GLASS]:{top:'glass',side:'glass',bottom:'glass',transparent:true},
  [BRICK]:{top:'brick',side:'brick',bottom:'brick'},
  [TNT]:{top:'tnt_top',side:'tnt_side',bottom:'tnt_bottom'},
};
const BLOCK_NAMES = {
  [GRASS]:'Grass',[DIRT]:'Dirt',[STONE]:'Stone',[WOOD]:'Wood',[LEAVES]:'Leaves',[SAND]:'Sand',
  [WATER_SOURCE]:'Water Source',[PLANKS]:'Planks',[COBBLE]:'Cobblestone',[GLASS]:'Glass',[BRICK]:'Brick',[TNT]:'TNT',
  [WATER_FLOW_1]:'Water', [WATER_FLOW_2]:'Water', [WATER_FLOW_3]:'Water',
  [PISTOL]:'Pistol', [BAZOOKA]:'Bazooka', 
  [TORCH]:'Torch', [TORCH_N]:'Torch', [TORCH_S]:'Torch', [TORCH_E]:'Torch', [TORCH_W]:'Torch'
};

const INVENTORY_CATS = {
  'all': [GRASS, DIRT, STONE, COBBLE, WOOD, PLANKS, LEAVES, SAND, GLASS, BRICK, WATER_SOURCE, TNT, TORCH, PISTOL, BAZOOKA],
  'building': [PLANKS, COBBLE, GLASS, BRICK, STONE, TORCH],
  'nature': [GRASS, DIRT, SAND, WOOD, LEAVES, WATER_SOURCE],
  'special': [TNT, TORCH],
  'guns': [PISTOL, BAZOOKA],
  'player': [] // Handled dynamically
};

const FACES = [
  {dir:[1,0,0], corners:[[1,0,1],[1,0,0],[1,1,0],[1,1,1]], uvs:[[0,0],[1,0],[1,1],[0,1]]},
  {dir:[-1,0,0], corners:[[0,0,0],[0,0,1],[0,1,1],[0,1,0]], uvs:[[0,0],[1,0],[1,1],[0,1]]},
  {dir:[0,1,0], corners:[[0,1,1],[1,1,1],[1,1,0],[0,1,0]], uvs:[[0,0],[1,0],[1,1],[0,1]]},
  {dir:[0,-1,0], corners:[[0,0,0],[1,0,0],[1,0,1],[0,0,1]], uvs:[[0,0],[1,0],[1,1],[0,1]]},
  {dir:[0,0,1], corners:[[0,0,1],[1,0,1],[1,1,1],[0,1,1]], uvs:[[0,0],[1,0],[1,1],[0,1]]},
  {dir:[0,0,-1], corners:[[1,0,0],[0,0,0],[0,1,0],[1,1,0]], uvs:[[0,0],[1,0],[1,1],[0,1]]},
];

const WATER_HEIGHTS = {
  [WATER_SOURCE]: 1.0,
  [WATER_FLOW_1]: 0.75,
  [WATER_FLOW_2]: 0.5,
  [WATER_FLOW_3]: 0.25
};

let HOTBAR = [GRASS, DIRT, STONE, COBBLE, WOOD, PLANKS, LEAVES, SAND, TNT];
let selectedSlot = 0;
let playerInv = new Array(27).fill(AIR);
let cursorItem = AIR;

// ============================ STATE ============================
let scene, camera, renderer, controls;
let worldData, atlasTexture, atlasCanvas;
let sun, hemiLight, sunMesh, stars;
let highlight;
const chunks = new Map();
const chunksToRebuild = new Set();
const particles = [];
const projectiles = [];
const shootingStars = [];
const activeTNT = []; 
const torchLights = []; 
let torchScanTimer = 0;
let shakeTime = 0, shakeIntensity = 0;
let audioCtx = null;
let dayTime = 0.28;
let lastTime = performance.now();
let mouseLeft = false, mouseRight = false;
let breakCD = 0, placeCD = 0;
let heldGroup, heldMesh = null;
let swingTime = 0;
let isInventoryOpen = false;
let selectedInvBlock = null;
let currentTab = 'all';
const keys = {};
const player = { pos: new THREE.Vector3(WORLD_SIZE/2, 30, WORLD_SIZE/2), vel: new THREE.Vector3(), onGround: false, inWater: false };
const stats = { fps: 0, frames: 0, fpsTime: 0 };
let gameStarted = false;
let gameState = 'MENU';
let settings = { fov: 75, sensitivity: 1.0, renderDistance: 72, fancyGraphics: true, seeThroughGlass: true, wavingLeaves: true, shootingStars: true };
let settingsReturnScreen = 'screen-main';
let solidMat, leafMat, glassMat, waterMat, tntMat, torchMat;
let composer, bloomPass, shaderPass;
let torchParticleGeo, torchMatRed, torchMatYellow;

let waterQueue = [];
let waterQueueSet = new Set();
let waterProcessTimer = 0;

// Custom Post Processing Shader
const ShaderGrading = {
  uniforms: {
    tDiffuse: { value: null },
    time: { value: 0 },
    resolution: { value: new THREE.Vector2(innerWidth, innerHeight) }
  },
  vertexShader: `
    varying vec2 vUv;
    void main() {
      vUv = uv;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
  `,
  fragmentShader: `
    uniform sampler2D tDiffuse;
    uniform float time;
    uniform vec2 resolution;
    varying vec2 vUv;
    
    float random(vec2 st) {
      return fract(sin(dot(st.xy, vec2(12.9898,78.233))) * 43758.5453123);
    }

    void main() {
      vec4 color = texture2D(tDiffuse, vUv);
      
      vec2 uv = vUv * (1.0 - vUv);
      float vignette = uv.x * uv.y * 10.0;
      vignette = clamp(vignette, 0.0, 1.0);
      color.rgb *= mix(0.8, 1.0, vignette);
      
      vec3 luminance = vec3(dot(color.rgb, vec3(0.299, 0.587, 0.114)));
      color.rgb = mix(luminance, color.rgb, 1.1); 
      
      color.rgb *= 1.1;
      
      float grain = random(vUv * resolution + time * 60.0);
      color.rgb += (grain - 0.5) * 0.02;
      
      gl_FragColor = color;
    }
  `
};

// ============================ NOISE & SEED ============================
let _seed = 0;
function parseSeed(seedStr) {
  if (!seedStr || seedStr.trim() === '') return Math.floor(Math.random() * 1000000);
  if (!isNaN(seedStr)) return parseInt(seedStr);
  let h = 0;
  for (let i = 0; i < seedStr.length; i++) {
    h = (h << 5) - h + seedStr.charCodeAt(i);
    h |= 0;
  }
  return Math.abs(h);
}

function hash2(x, y) {
  let h = ((x * 374761393) + (y * 668265263) + _seed * 1013) | 0;
  h = (h ^ (h >> 13)) * 1274126177;
  return ((h ^ (h >> 16)) >>> 0) / 4294967295;
}
function noise2D(x, y) {
  const x0 = Math.floor(x), y0 = Math.floor(y);
  const sx = x - x0, sy = y - y0;
  const n00 = hash2(x0,y0), n10 = hash2(x0+1,y0), n01 = hash2(x0,y0+1), n11 = hash2(x0+1,y0+1);
  const fx = sx*sx*(3-2*sx), fy = sy*sy*(3-2*sy);
  return (n00 + (n10-n00)*fx) + ((n01 + (n11-n01)*fx) - (n00 + (n10-n00)*fx)) * fy;
}
function fbm(x, y, octaves=4) {
  let val = 0, amp = 1, freq = 1, max = 0;
  for (let i = 0; i < octaves; i++) { val += noise2D(x*freq, y*freq) * amp; max += amp; amp *= 0.5; freq *= 2; }
  return val / max;
}

// ============================ WORLD ACCESS ============================
function wIdx(x, y, z) { return y * WORLD_SIZE * WORLD_SIZE + z * WORLD_SIZE + x; }
function getBlock(x, y, z) {
  x = Math.floor(x); y = Math.floor(y); z = Math.floor(z);
  if (x < 0 || x >= WORLD_SIZE || y < 0 || y >= WORLD_HEIGHT || z < 0 || z >= WORLD_SIZE) return AIR;
  return worldData[wIdx(x, y, z)];
}

function addWaterUpdate(x, y, z) {
  if (x < 0 || x >= WORLD_SIZE || y < 0 || y >= WORLD_HEIGHT || z < 0 || z >= WORLD_SIZE) return;
  const key = x + ',' + y + ',' + z;
  if (!waterQueueSet.has(key)) {
    waterQueue.push(key);
    waterQueueSet.add(key);
  }
}

function setBlock(x, y, z, v) {
  x = Math.floor(x); y = Math.floor(y); z = Math.floor(z);
  if (x < 0 || x >= WORLD_SIZE || y < 0 || y >= WORLD_HEIGHT || z < 0 || z >= WORLD_SIZE) return;
  worldData[wIdx(x, y, z)] = v;
  
  const cx = Math.floor(x / CHUNK_SIZE), cz = Math.floor(z / CHUNK_SIZE);
  chunksToRebuild.add(cx + ',' + cz);
  if (x % CHUNK_SIZE === 0 && cx > 0) chunksToRebuild.add((cx-1) + ',' + cz);
  if (x % CHUNK_SIZE === CHUNK_SIZE-1 && cx < CHUNKS_PER_SIDE-1) chunksToRebuild.add((cx+1) + ',' + cz);
  if (z % CHUNK_SIZE === 0 && cz > 0) chunksToRebuild.add(cx + ',' + (cz-1));
  if (z % CHUNK_SIZE === CHUNK_SIZE-1 && cz < CHUNKS_PER_SIDE-1) chunksToRebuild.add(cx + ',' + (cz+1));

  addWaterUpdate(x, y, z);
  addWaterUpdate(x+1, y, z); addWaterUpdate(x-1, y, z);
  addWaterUpdate(x, y+1, z); addWaterUpdate(x, y-1, z);
  addWaterUpdate(x, y, z+1); addWaterUpdate(x, y, z-1);
}

function isTorch(t) { return t === TORCH || t === TORCH_N || t === TORCH_S || t === TORCH_E || t === TORCH_W; }
function isWater(t) { return t === WATER_SOURCE || t === WATER_FLOW_1 || t === WATER_FLOW_2 || t === WATER_FLOW_3; }
function getWaterLevel(t) {
  if (t === WATER_SOURCE) return 0;
  if (t === WATER_FLOW_1) return 1;
  if (t === WATER_FLOW_2) return 2;
  if (t === WATER_FLOW_3) return 3;
  return -1;
}
function getWaterHeight(t) { return WATER_HEIGHTS[t] || 0; }
function isSolid(t) { return t !== AIR && !isWater(t) && !isTorch(t); }
function isTransparent(t) { return t === AIR || isWater(t) || t === GLASS || t === LEAVES || isTorch(t); }

// ============================ WATER FLOW LOGIC ============================
function processWaterQueue(dt) {
  waterProcessTimer += dt;
  if (waterProcessTimer < 0.1) return;
  waterProcessTimer = 0;

  const batchSize = Math.min(200, waterQueue.length);
  for (let i = 0; i < batchSize; i++) {
    const key = waterQueue.shift();
    waterQueueSet.delete(key);
    const [x, y, z] = key.split(',').map(Number);
    updateWaterBlock(x, y, z);
  }
}

function updateWaterBlock(x, y, z) {
  const b = getBlock(x, y, z);
  const lvl = getWaterLevel(b);
  if (lvl === -1) return;

  let sustained = (lvl === 0);
  if (getBlock(x, y + 1, z) !== AIR && isWater(getBlock(x, y + 1, z))) sustained = true;

  const dirs = [[1, 0], [-1, 0], [0, 1], [0, -1]];
  if (!sustained) {
    for (let i = 0; i < 4; i++) {
      const nb = getBlock(x + dirs[i][0], y, z + dirs[i][1]);
      const nbLvl = getWaterLevel(nb);
      if (nbLvl !== -1 && nbLvl < lvl) sustained = true;
    }
  }

  if (!sustained) {
    setBlock(x, y, z, AIR);
    return;
  }

  const below = getBlock(x, y - 1, z);
  if (below === AIR) {
    if (b === WATER_SOURCE) setBlock(x, y - 1, z, WATER_FLOW_1);
    else setBlock(x, y - 1, z, b);
    return;
  }

  if (lvl < 3) {
    const newLvl = lvl === 0 ? WATER_FLOW_1 : (lvl === 1 ? WATER_FLOW_2 : WATER_FLOW_3);
    for (let i = 0; i < 4; i++) {
      const nx = x + dirs[i][0], nz = z + dirs[i][1];
      if (getBlock(nx, y, nz) === AIR) setBlock(nx, y, nz, newLvl);
    }
  }
}

// ============================ SAVE / LOAD ============================
function saveGame() {
  if (!gameStarted) return;
  let binary = '';
  const len = worldData.byteLength;
  for (let i = 0; i < len; i++) binary += String.fromCharCode(worldData[i]);
  
  const saveData = {
    world: btoa(binary),
    player: { x: player.pos.x, y: player.pos.y, z: player.pos.z },
    hotbar: HOTBAR,
    playerInv: playerInv,
    selectedSlot: selectedSlot,
    dayTime: dayTime,
    seed: _seed
  };
  localStorage.setItem('minecraft_save', JSON.stringify(saveData));
  showToast('Game Saved!');
}

function loadGame() {
  const data = localStorage.getItem('minecraft_save');
  if (!data) {
    showToast('No Save Found!');
    return false;
  }
  try {
    const save = JSON.parse(data);
    _seed = save.seed;
    worldData = new Uint8Array(atob(save.world).split('').map(c => c.charCodeAt(0)));
    player.pos.set(save.player.x, save.player.y, save.player.z);
    player.vel.set(0, 0, 0);
    HOTBAR = save.hotbar;
    playerInv = save.playerInv || new Array(27).fill(AIR);
    selectedSlot = save.selectedSlot;
    dayTime = save.dayTime;
    
    chunks.forEach(chunk => {
      if (chunk.solid) { scene.remove(chunk.solid); chunk.solid.geometry.dispose(); }
      if (chunk.leaf) { scene.remove(chunk.leaf); chunk.leaf.geometry.dispose(); }
      if (chunk.glass) { scene.remove(chunk.glass); chunk.glass.geometry.dispose(); }
      if (chunk.tnt) { scene.remove(chunk.tnt); chunk.tnt.geometry.dispose(); }
      if (chunk.torch) { scene.remove(chunk.torch); chunk.torch.geometry.dispose(); }
      if (chunk.water) { scene.remove(chunk.water); chunk.water.geometry.dispose(); }
    });
    chunks.clear();
    chunksToRebuild.clear();
    
    for (let cx = 0; cx < CHUNKS_PER_SIDE; cx++)
      for (let cz = 0; cz < CHUNKS_PER_SIDE; cz++)
        chunksToRebuild.add(cx + ',' + cz);
        
    waterQueue = [];
    waterQueueSet = new Set();
    for (let x = 0; x < WORLD_SIZE; x++) {
      for (let y = 0; y < WORLD_HEIGHT; y++) {
        for (let z = 0; z < WORLD_SIZE; z++) {
          if (isWater(getBlock(x, y, z))) addWaterUpdate(x, y, z);
        }
      }
    }
    
    controls.getObject().position.set(player.pos.x, player.pos.y + PLAYER_EYE, player.pos.z);
    
    setupHotbar();
    updateHeldItem();
    return true;
  } catch (e) {
    console.error("Load failed", e);
    showToast('Save Corrupted!');
    return false;
  }
}

// ============================ INIT ============================
init();

function init() {
  const savedSettings = localStorage.getItem('minecraft_settings');
  if (savedSettings) {
    try { settings = { ...settings, ...JSON.parse(savedSettings) }; } catch(e) {}
  }

  scene = new THREE.Scene();
  scene.background = new THREE.Color(0x87ceeb);
  scene.fog = new THREE.Fog(0x87ceeb, 32, settings.renderDistance);

  camera = new THREE.PerspectiveCamera(settings.fov, innerWidth / innerHeight, 0.1, 250);

  renderer = new THREE.WebGLRenderer({ antialias: false });
  renderer.setSize(innerWidth, innerHeight);
  renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
  renderer.outputColorSpace = THREE.SRGBColorSpace;
  document.body.appendChild(renderer.domElement);

  // Apply Tone Mapping initially
  renderer.toneMapping = settings.fancyGraphics ? THREE.ACESFilmicToneMapping : THREE.NoToneMapping;
  renderer.toneMappingExposure = settings.fancyGraphics ? 1.4 : 1.0;

  // Post Processing Setup
  composer = new EffectComposer(renderer);
  composer.addPass(new RenderPass(scene, camera));
  
  // Bloom Pass
  bloomPass = new UnrealBloomPass(new THREE.Vector2(innerWidth, innerHeight), 0.8, 0.4, 0.85);
  composer.addPass(bloomPass);
  
  // Custom Color Grading Pass
  shaderPass = new ShaderPass(ShaderGrading);
  composer.addPass(shaderPass);

  hemiLight = new THREE.HemisphereLight(0xb0d8ff, 0x4a3a25, 0.8);
  scene.add(hemiLight);

  sun = new THREE.DirectionalLight(0xffffff, 1.5);
  sun.position.set(50, 80, 30);
  scene.add(sun);
  scene.add(sun.target);

  // Dynamic Torch Lights
  for (let i = 0; i < 3; i++) {
    const light = new THREE.PointLight(0xffaa55, 0, 15, 2);
    scene.add(light);
    torchLights.push({ light, pos: new THREE.Vector3() });
  }

  sunMesh = new THREE.Mesh(new THREE.SphereGeometry(4, 16, 16), new THREE.MeshBasicMaterial({ color: 0xffee88, fog: false }));
  scene.add(sunMesh);

  const starGeo = new THREE.BufferGeometry();
  const starPos = [];
  for (let i = 0; i < 400; i++) {
    const theta = Math.random() * Math.PI * 2;
    const phi = Math.acos(Math.random() * 0.6 + 0.2);
    const r = 180;
    starPos.push(r * Math.sin(phi) * Math.cos(theta), r * Math.cos(phi), r * Math.sin(phi) * Math.sin(theta));
  }
  starGeo.setAttribute('position', new THREE.Float32BufferAttribute(starPos, 3));
  stars = new THREE.Points(starGeo, new THREE.PointsMaterial({ color: 0xffffff, size: 1.5, sizeAttenuation: false, fog: false, transparent: true, opacity: 0 }));
  scene.add(stars);

  generateAtlas();
  updateMaterials();

  // Torch Particles Setup
  torchParticleGeo = new THREE.BoxGeometry(0.06, 0.06, 0.06);
  torchMatRed = new THREE.MeshBasicMaterial({ color: 0xff4500, transparent: true, fog: true });
  torchMatYellow = new THREE.MeshBasicMaterial({ color: 0xffd700, transparent: true, fog: true });

  const hlGeo = new THREE.BoxGeometry(1.005, 1.005, 1.005);
  highlight = new THREE.LineSegments(new THREE.EdgesGeometry(hlGeo), new THREE.LineBasicMaterial({ color: 0x000000, transparent: true, opacity: 0.5 }));
  highlight.visible = false;
  scene.add(highlight);

  controls = new PointerLockControls(camera, renderer.domElement);
  scene.add(controls.getObject());
  controls.getObject().position.set(player.pos.x, player.pos.y + PLAYER_EYE, player.pos.z);
  controls.pointerSpeed = settings.sensitivity; 

  controls.addEventListener('lock', () => {
    hideAllMenus();
    gameState = 'PLAYING';
    document.getElementById('crosshair').style.display = 'block';
    document.getElementById('hotbar').style.display = 'flex';
    document.getElementById('stats').style.display = 'block';
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  });
  
  controls.addEventListener('unlock', () => {
    if (isInventoryOpen) return; 
    if (gameState === 'PLAYING') {
      gameState = 'PAUSED';
      document.getElementById('crosshair').style.display = 'none';
      document.getElementById('hotbar').style.display = 'none';
      document.getElementById('stats').style.display = 'none';
      showMenuScreen('screen-pause');
    }
  });

  heldGroup = new THREE.Group();
  heldGroup.position.set(0.55, -0.45, -0.7);
  heldGroup.rotation.set(0.1, -0.3, 0);
  camera.add(heldGroup);
  scene.add(camera);
  updateHeldItem();

  setupInput();
  setupHotbar();
  setupInventory();
  setupMenus();
  addEventListener('resize', onResize);

  // Apply settings to UI
  document.getElementById('slider-sens').value = settings.sensitivity;
  document.getElementById('val-sens').textContent = settings.sensitivity.toFixed(1);
  document.getElementById('slider-fov').value = settings.fov;
  document.getElementById('val-fov').textContent = settings.fov;
  document.getElementById('slider-render').value = settings.renderDistance;
  document.getElementById('val-render').textContent = settings.renderDistance;
  
  const setupToggle = (id, key) => {
    const btn = document.getElementById(id);
    btn.classList.toggle('on', settings[key]);
    btn.classList.toggle('off', !settings[key]);
    btn.textContent = settings[key] ? 'ON' : 'OFF';
  };
  setupToggle('toggle-fancy', 'fancyGraphics');
  setupToggle('toggle-glass', 'seeThroughGlass');
  setupToggle('toggle-leaves', 'wavingLeaves');
  setupToggle('toggle-stars', 'shootingStars');

  animate();
}

// ============================ MENU LOGIC ============================
function showMenuScreen(id) {
  document.querySelectorAll('.menu-screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  document.getElementById('menu-container').style.display = 'flex';
}
function hideAllMenus() {
  document.querySelectorAll('.menu-screen').forEach(s => s.classList.remove('active'));
  document.getElementById('menu-container').style.display = 'none';
}

function startWorldGeneration(seedStr) {
  showMenuScreen('screen-loading');
  document.getElementById('btn-enter-game').style.display = 'none';
  document.getElementById('load-fill').style.width = '0%';
  document.getElementById('load-text').textContent = '0%';

  setTimeout(() => {
    chunks.forEach(chunk => {
      if (chunk.solid) { scene.remove(chunk.solid); chunk.solid.geometry.dispose(); }
      if (chunk.leaf) { scene.remove(chunk.leaf); chunk.leaf.geometry.dispose(); }
      if (chunk.glass) { scene.remove(chunk.glass); chunk.glass.geometry.dispose(); }
      if (chunk.tnt) { scene.remove(chunk.tnt); chunk.tnt.geometry.dispose(); }
      if (chunk.torch) { scene.remove(chunk.torch); chunk.torch.geometry.dispose(); }
      if (chunk.water) { scene.remove(chunk.water); chunk.water.geometry.dispose(); }
    });
    chunks.clear();
    chunksToRebuild.clear();
    activeTNT.length = 0;
    projectiles.length = 0;

    _seed = parseSeed(seedStr);
    generateWorld();
    
    for (let cx = 0; cx < CHUNKS_PER_SIDE; cx++)
      for (let cz = 0; cz < CHUNKS_PER_SIDE; cz++)
        chunksToRebuild.add(cx + ',' + cz);
      
    const sx = Math.floor(WORLD_SIZE/2), sz = Math.floor(WORLD_SIZE/2);
    for (let y = WORLD_HEIGHT-1; y >= 0; y--) {
      const b = getBlock(sx, y, sz);
      if (b !== AIR && !isWater(b) && b !== LEAVES && b !== WOOD) {
        player.pos.set(sx + 0.5, y + 1, sz + 0.5);
        break;
      }
    }
    controls.getObject().position.set(player.pos.x, player.pos.y + PLAYER_EYE, player.pos.z);
    HOTBAR = [GRASS, DIRT, STONE, COBBLE, WOOD, PLANKS, LEAVES, SAND, TNT];
    playerInv = new Array(27).fill(AIR);
    selectedSlot = 0;
    setupHotbar();
    updateHeldItem();
    
    gameState = 'LOADING';
  }, 50);
}

function setupMenus() {
  document.getElementById('btn-new-main').addEventListener('click', () => showMenuScreen('screen-new'));
  document.getElementById('btn-load-main').addEventListener('click', () => {
    if (loadGame()) {
      startWorldGeneration(null); 
    }
  });
  document.getElementById('btn-settings-main').addEventListener('click', () => {
    settingsReturnScreen = 'screen-main';
    showMenuScreen('screen-settings');
  });

  document.getElementById('btn-create-world').addEventListener('click', () => {
    const seedStr = document.getElementById('seed-input').value;
    startWorldGeneration(seedStr);
  });
  document.getElementById('btn-cancel-new').addEventListener('click', () => showMenuScreen('screen-main'));

  document.getElementById('btn-enter-game').addEventListener('click', () => {
    gameStarted = true;
    controls.lock(); 
  });

  document.getElementById('slider-sens').addEventListener('input', (e) => {
    settings.sensitivity = parseFloat(e.target.value);
    document.getElementById('val-sens').textContent = settings.sensitivity.toFixed(1);
    controls.pointerSpeed = settings.sensitivity;
  });
  document.getElementById('slider-fov').addEventListener('input', (e) => {
    settings.fov = parseInt(e.target.value);
    document.getElementById('val-fov').textContent = settings.fov;
    camera.fov = settings.fov;
    camera.updateProjectionMatrix();
  });
  document.getElementById('slider-render').addEventListener('input', (e) => {
    settings.renderDistance = parseInt(e.target.value);
    document.getElementById('val-render').textContent = settings.renderDistance;
    scene.fog.far = settings.renderDistance;
  });
  
  const addToggleListener = (id, key, callback) => {
    document.getElementById(id).addEventListener('click', (e) => {
      settings[key] = !settings[key];
      e.target.classList.toggle('on', settings[key]);
      e.target.classList.toggle('off', !settings[key]);
      e.target.textContent = settings[key] ? 'ON' : 'OFF';
      if (callback) callback();
    });
  };
  
  addToggleListener('toggle-fancy', 'fancyGraphics', () => {
    renderer.toneMapping = settings.fancyGraphics ? THREE.ACESFilmicToneMapping : THREE.NoToneMapping;
    renderer.toneMappingExposure = settings.fancyGraphics ? 1.4 : 1.0;
  });
  
  const rebuildChunks = () => {
    updateMaterials();
    chunks.forEach(chunk => {
      if (chunk.solid) { scene.remove(chunk.solid); chunk.solid.geometry.dispose(); }
      if (chunk.leaf) { scene.remove(chunk.leaf); chunk.leaf.geometry.dispose(); }
      if (chunk.glass) { scene.remove(chunk.glass); chunk.glass.geometry.dispose(); }
      if (chunk.tnt) { scene.remove(chunk.tnt); chunk.tnt.geometry.dispose(); }
      if (chunk.torch) { scene.remove(chunk.torch); chunk.torch.geometry.dispose(); }
      if (chunk.water) { scene.remove(chunk.water); chunk.water.geometry.dispose(); }
    });
    chunks.clear();
    for (let cx = 0; cx < CHUNKS_PER_SIDE; cx++)
      for (let cz = 0; cz < CHUNKS_PER_SIDE; cz++)
        chunksToRebuild.add(cx + ',' + cz);
  };
  
  addToggleListener('toggle-glass', 'seeThroughGlass', rebuildChunks);
  addToggleListener('toggle-leaves', 'wavingLeaves', rebuildChunks);
  addToggleListener('toggle-stars', 'shootingStars');

  document.getElementById('btn-back-settings').addEventListener('click', () => {
    localStorage.setItem('minecraft_settings', JSON.stringify(settings));
    showMenuScreen(settingsReturnScreen);
  });

  document.getElementById('btn-resume').addEventListener('click', () => controls.lock());
  document.getElementById('btn-settings-pause').addEventListener('click', () => {
    settingsReturnScreen = 'screen-pause';
    showMenuScreen('screen-settings');
  });
  document.getElementById('btn-save-game').addEventListener('click', saveGame);
  document.getElementById('btn-quit-game').addEventListener('click', () => {
    gameStarted = false;
    gameState = 'MENU';
    showMenuScreen('screen-main');
  });
}

// ============================ ATLAS & MATERIALS ============================
function generateAtlas() {
  const size = TILE_PX * ATLAS_N;
  atlasCanvas = document.createElement('canvas');
  atlasCanvas.width = size; atlasCanvas.height = size;
  const ctx = atlasCanvas.getContext('2d');
  ctx.imageSmoothingEnabled = false;

  function fill(tx, ty, c) { ctx.fillStyle = c; ctx.fillRect(tx*TILE_PX, ty*TILE_PX, TILE_PX, TILE_PX); }
  function noiseFill(tx, ty, colors, density=45) {
    const ox = tx*TILE_PX, oy = ty*TILE_PX;
    for (let i = 0; i < density; i++) {
      ctx.fillStyle = colors[Math.floor(Math.random()*colors.length)];
      ctx.fillRect(ox + Math.floor(Math.random()*TILE_PX), oy + Math.floor(Math.random()*TILE_PX), 1, 1);
    }
  }

  fill(0,0,'#6aa84e'); noiseFill(0,0,['#5d9543','#7cb95d','#548a3c','#82c25e'],55);
  fill(1,0,'#7a5b3a'); noiseFill(1,0,['#6b4d2e','#8a6840'],30);
  ctx.fillStyle = '#6aa84e'; ctx.fillRect(16,0,16,4);
  for (let x = 0; x < 16; x++) { if (Math.random()>0.35) ctx.fillRect(16+x,4,1,1); if (Math.random()>0.7) ctx.fillRect(16+x,5,1,1); }
  ctx.fillStyle = '#5d9543'; for (let i = 0; i < 10; i++) ctx.fillRect(16+Math.floor(Math.random()*16), Math.floor(Math.random()*4), 1, 1);
  fill(2,0,'#7a5b3a'); noiseFill(2,0,['#6b4d2e','#8a6840','#5a4528'],55);
  fill(3,0,'#888888'); noiseFill(3,0,['#777','#999','#6a6a6a','#a0a0a0'],55);
  ctx.strokeStyle = '#555'; ctx.lineWidth = 1; ctx.beginPath(); ctx.moveTo(51,5); ctx.lineTo(55,9); ctx.lineTo(54,13); ctx.stroke();
  
  // Torch (4,0) - Left blank, shader handles the colors
  ctx.clearRect(64, 0, 16, 16);

  fill(0,1,'#9c8053'); ctx.strokeStyle = '#7a6038'; ctx.lineWidth = 1;
  ctx.beginPath(); ctx.arc(8,24,2,0,Math.PI*2); ctx.stroke();
  ctx.beginPath(); ctx.arc(8,24,5,0,Math.PI*2); ctx.stroke();
  ctx.beginPath(); ctx.arc(8,24,7,0,Math.PI*2); ctx.stroke();
  ctx.fillStyle = '#6b5028'; ctx.fillRect(7,23,2,2);
  fill(1,1,'#6b5028');
  for (let x = 0; x < 16; x += 2) { ctx.fillStyle = x%4===0 ? '#5a4220' : '#7a6038'; ctx.fillRect(16+x,16,1,16); }
  noiseFill(1,1,['#5a4220','#8a7048','#4a3210'],25);
  fill(2,1,'#3a6a25'); noiseFill(2,1,['#2d5a1a','#4a7a35','#1d4a10','#5a8a40'],70);
  fill(3,1,'#e6d4a3'); noiseFill(3,1,['#d4c290','#f0e0b8','#c8b478'],45);

  fill(0,2,'#3a78c8'); noiseFill(0,2,['#2a68b8','#4a88d8','#5a98e8'],35);
  fill(1,2,'#b88848');
  ctx.fillStyle = '#9a7038'; ctx.fillRect(16,20,16,1); ctx.fillRect(16,27,16,1);
  ctx.fillStyle = '#c89858'; ctx.fillRect(16,21,16,1); ctx.fillRect(16,28,16,1);
  noiseFill(1,2,['#a87838','#c89455'],20);
  fill(2,2,'#7a7a7a');
  ctx.fillStyle = '#666'; ctx.fillRect(32,16,7,5); ctx.fillRect(40,16,7,5); ctx.fillRect(32,22,5,5); ctx.fillRect(38,22,9,5); ctx.fillRect(32,28,7,4); ctx.fillRect(40,28,7,4);
  ctx.fillStyle = '#999'; ctx.fillRect(33,17,5,3); ctx.fillRect(41,17,5,3); ctx.fillRect(33,23,3,3); ctx.fillRect(39,23,7,3); ctx.fillRect(33,29,5,2); ctx.fillRect(41,29,5,2);
  noiseFill(2,2,['#5a5a5a','#8a8a8a'],15);
  fill(3,2,'#c8e8f0');
  ctx.strokeStyle = '#a0c8d8'; ctx.lineWidth = 1;
  ctx.strokeRect(48.5,32.5,15,15);
  ctx.fillStyle = 'rgba(255,255,255,0.4)'; ctx.fillRect(50,34,4,4); ctx.fillRect(58,40,3,3);
  fill(0,3,'#9a4a3a');
  ctx.fillStyle = '#7a3a2a'; ctx.fillRect(0,48,16,1); ctx.fillRect(0,55,16,1); ctx.fillRect(0,62,16,1);
  ctx.fillRect(7,48,1,7); ctx.fillRect(3,55,1,7); ctx.fillRect(11,55,1,7); ctx.fillRect(7,62,1,7);
  noiseFill(0,3,['#8a3a2a','#aa5a4a'],20);
  fill(1,3,'#aa3030'); noiseFill(1,3,['#882020', '#cc4040'], 40);
  ctx.fillStyle = '#551010'; ctx.beginPath(); ctx.arc(24, 56, 4, 0, Math.PI*2); ctx.fill();
  fill(2,3,'#aa3030'); noiseFill(2,3,['#882020', '#cc4040'], 40);
  ctx.fillStyle = '#ffffff'; 
  ctx.fillRect(34, 48, 12, 2); ctx.fillRect(34, 50, 2, 4); ctx.fillRect(44, 50, 2, 4); ctx.fillRect(34, 54, 12, 2);
  ctx.fillStyle = '#000000'; ctx.fillRect(36, 50, 2, 4); ctx.fillRect(42, 50, 2, 4); ctx.fillRect(39, 51, 1, 2);
  fill(3,3,'#771818'); noiseFill(3,3,['#551010', '#992020'], 40);

  atlasTexture = new THREE.CanvasTexture(atlasCanvas);
  atlasTexture.magFilter = THREE.NearestFilter;
  atlasTexture.minFilter = THREE.NearestFilter;
  atlasTexture.generateMipmaps = false;
  atlasTexture.colorSpace = THREE.SRGBColorSpace;
}

function updateMaterials() {
  if (!atlasTexture) return;
  solidMat = new THREE.MeshLambertMaterial({ map: atlasTexture, alphaTest: 0.1 });
  
  // Subtle TNT Emissive
  tntMat = new THREE.MeshLambertMaterial({ map: atlasTexture, emissive: 0x331010, emissiveIntensity: 0.5 });

  // Emissive Torch Material with Custom Shader
  torchMat = new THREE.MeshBasicMaterial({ fog: true });
  torchMat.onBeforeCompile = (shader) => {
    shader.uniforms.time = { value: 0 };
    shader.vertexShader = 'varying vec2 vTorchUv;\n' + shader.vertexShader;
    shader.vertexShader = shader.vertexShader.replace(
      '#include <uv_vertex>',
      `#include <uv_vertex>
       vTorchUv = uv;`
    );
    shader.fragmentShader = 'varying vec2 vTorchUv;\nuniform float time;\n' + shader.fragmentShader;
    shader.fragmentShader = shader.fragmentShader.replace(
      '#include <color_fragment>',
      `
      #include <color_fragment>
      if (vTorchUv.y > 0.4) {
          float t = time * 6.0 + vTorchUv.x * 10.0 + vTorchUv.y * 5.0;
          float flicker = sin(t) * 0.5 + 0.5;
          float fireHeight = (vTorchUv.y - 0.4) / 0.6;
          vec3 color1 = vec3(1.0, 0.2, 0.0); // Red
          vec3 color2 = vec3(1.0, 0.8, 0.0); // Yellow
          float mixFactor = clamp(fireHeight + flicker * 0.3, 0.0, 1.0);
          vec3 fireColor = mix(color2, color1, mixFactor);
          diffuseColor.rgb = fireColor;
      } else {
          diffuseColor.rgb = vec3(0.20, 0.12, 0.05); // Darker Browner
      }
      `
    );
    torchMat.userData.shader = shader;
  };

  leafMat = new THREE.MeshLambertMaterial({ map: atlasTexture, alphaTest: 0.5 });
  if (settings.wavingLeaves) {
    leafMat.onBeforeCompile = (shader) => {
      shader.uniforms.time = { value: 0 };
      shader.vertexShader = 'uniform float time;\n' + shader.vertexShader;
      shader.vertexShader = shader.vertexShader.replace(
        '#include <begin_vertex>',
        `vec3 transformed = vec3(position);
         transformed.x += sin(time * 2.0 + position.y * 2.0) * 0.05;
         transformed.z += cos(time * 1.5 + position.x * 2.0) * 0.05;`
      );
      leafMat.userData.shader = shader;
    };
  }

  // Reflective Glass Material
  glassMat = new THREE.MeshPhongMaterial({ 
    map: atlasTexture, 
    transparent: settings.seeThroughGlass, 
    opacity: settings.seeThroughGlass ? 0.6 : 1.0, 
    depthWrite: !settings.seeThroughGlass,
    alphaTest: settings.seeThroughGlass ? 0.0 : 0.3,
    shininess: 100, 
    specular: 0xaaaaaa 
  });

  // Enhanced Water Material
  waterMat = new THREE.MeshLambertMaterial({ 
    map: atlasTexture, 
    transparent: true, 
    opacity: 0.75, 
    depthWrite: false
  });
}

// ============================ WORLD GEN ============================
function generateWorld() {
  worldData = new Uint8Array(WORLD_SIZE * WORLD_HEIGHT * WORLD_SIZE);
  waterQueue = [];
  waterQueueSet = new Set();

  for (let x = 0; x < WORLD_SIZE; x++) {
    for (let z = 0; z < WORLD_SIZE; z++) {
      const nx = x / 18, nz = z / 18;
      let h = SEA_LEVEL + fbm(nx, nz, 4) * 14 - 2;
      h = Math.floor(h);
      if (h < 1) h = 1; if (h > WORLD_HEIGHT-8) h = WORLD_HEIGHT-8;

      for (let y = 0; y <= h; y++) {
        let bt;
        if (y === h) {
          if (h <= SEA_LEVEL + 1) bt = SAND;
          else bt = GRASS;
        } else if (y >= h - 3) {
          if (h <= SEA_LEVEL + 1) bt = SAND;
          else bt = DIRT;
        } else bt = STONE;
        worldData[wIdx(x, y, z)] = bt;
      }

      for (let y = h + 1; y <= SEA_LEVEL; y++) {
        if (worldData[wIdx(x, y, z)] === AIR) {
          worldData[wIdx(x, y, z)] = WATER_SOURCE;
          addWaterUpdate(x, y, z);
        }
      }
    }
  }

  for (let i = 0; i < 40; i++) {
    const tx = 3 + Math.floor(Math.random() * (WORLD_SIZE - 6));
    const tz = 3 + Math.floor(Math.random() * (WORLD_SIZE - 6));
    let groundY = -1;
    for (let y = WORLD_HEIGHT - 1; y >= 0; y--) {
      if (getBlock(tx, y, tz) === GRASS) { groundY = y; break; }
    }
    if (groundY < 0) continue;
    const h = 4 + Math.floor(Math.random() * 3);
    for (let y = 1; y <= h; y++) worldData[wIdx(tx, groundY + y, tz)] = WOOD;
    const ly = groundY + h;
    for (let dx = -2; dx <= 2; dx++)
      for (let dz = -2; dz <= 2; dz++)
        for (let dy = -1; dy <= 2; dy++) {
          const r = Math.abs(dx) + Math.abs(dz) + Math.abs(dy);
          if (r > 3) continue;
          if (dx === 0 && dz === 0 && dy <= 0) continue;
          const x = tx+dx, y = ly+dy, z = tz+dz;
          if (x<0||x>=WORLD_SIZE||y<0||y>=WORLD_HEIGHT||z<0||z>=WORLD_SIZE) continue;
          if (getBlock(x, y, z) === AIR) worldData[wIdx(x, y, z)] = LEAVES;
        }
  }
}

// ============================ TORCH MESHING ============================
function addTiltedBox(cx, cy, cz, bx1, bx2, y1, y2, bz1, bz2, tx1, tx2, tz1, tz2, posArr, normArr, uvArr, idxArr, viRef, uvV0, uvV1) {
  const v = [
    [bx1, y1, bz1], [bx2, y1, bz1], [bx2, y1, bz2], [bx1, y1, bz2],
    [tx1, y2, tz1], [tx2, y2, tz1], [tx2, y2, tz2], [tx1, y2, tz2]
  ];

  const faces = [
    { corners: [1, 2, 6, 5], normal: [1, 0, 0] },
    { corners: [3, 0, 4, 7], normal: [-1, 0, 0] },
    { corners: [2, 3, 7, 6], normal: [0, 0, 1] },
    { corners: [0, 1, 5, 4], normal: [0, 0, -1] },
    { corners: [4, 5, 6, 7], normal: [0, 1, 0] },
    { corners: [3, 2, 1, 0], normal: [0, -1, 0] }
  ];

  for (const face of faces) {
    for (const ci of face.corners) {
      posArr.push(cx + v[ci][0], cy + v[ci][1], cz + v[ci][2]);
      normArr.push(face.normal[0], face.normal[1], face.normal[2]);
    }
    uvArr.push(0, uvV0, 1, uvV0, 1, uvV1, 0, uvV1);
    idxArr.push(viRef.vi, viRef.vi+1, viRef.vi+2, viRef.vi, viRef.vi+2, viRef.vi+3);
    viRef.vi += 4;
  }
}

function addTorch(cx, cy, cz, dir, posArr, normArr, uvArr, idxArr, viRef) {
  const u = 1/16;
  let s = {}, f = {};
  s.y1 = 0; s.y2 = 9*u;
  f.y1 = 9*u; f.y2 = 13*u;

  if (dir === 'UP') {
    s.x1=7*u; s.x2=9*u; s.z1=7*u; s.z2=9*u; s.tx1=7*u; s.tx2=9*u; s.tz1=7*u; s.tz2=9*u;
    f.x1=5*u; f.x2=11*u; f.z1=5*u; f.z2=11*u; f.tx1=5*u; f.tx2=11*u; f.tz1=5*u; f.tz2=11*u;
  } else if (dir === 'SOUTH') { // Wall at Z=0
    s.x1=7*u; s.x2=9*u; s.z1=0; s.z2=2*u; s.tx1=7*u; s.tx2=9*u; s.tz1=6*u; s.tz2=8*u;
    f.x1=5*u; f.x2=11*u; f.z1=4*u; f.z2=10*u; f.tx1=5*u; f.tx2=11*u; f.tz1=4*u; f.tz2=10*u;
  } else if (dir === 'NORTH') { // Wall at Z=16*u
    s.x1=7*u; s.x2=9*u; s.z1=14*u; s.z2=16*u; s.tx1=7*u; s.tx2=9*u; s.tz1=8*u; s.tz2=10*u;
    f.x1=5*u; f.x2=11*u; f.z1=6*u; f.z2=12*u; f.tx1=5*u; f.tx2=11*u; f.tz1=6*u; f.tz2=12*u;
  } else if (dir === 'EAST') { // Wall at X=0
    s.x1=0; s.x2=2*u; s.z1=7*u; s.z2=9*u; s.tx1=6*u; s.tx2=8*u; s.tz1=7*u; s.tz2=9*u;
    f.x1=4*u; f.x2=10*u; f.z1=5*u; f.z2=11*u; f.tx1=4*u; f.tx2=10*u; f.tz1=5*u; f.tz2=11*u;
  } else if (dir === 'WEST') { // Wall at X=16*u
    s.x1=14*u; s.x2=16*u; s.z1=7*u; s.z2=9*u; s.tx1=8*u; s.tx2=10*u; s.tz1=7*u; s.tz2=9*u;
    f.x1=6*u; f.x2=12*u; f.z1=5*u; f.z2=11*u; f.tx1=6*u; f.tx2=12*u; f.tz1=5*u; f.tz2=11*u;
  }

  addTiltedBox(cx, cy, cz, s.x1, s.x2, s.y1, s.y2, s.z1, s.z2, s.tx1, s.tx2, s.tz1, s.tz2, posArr, normArr, uvArr, idxArr, viRef, 0.0, 0.4);
  addTiltedBox(cx, cy, cz, f.x1, f.x2, f.y1, f.y2, f.z1, f.z2, f.tx1, f.tx2, f.tz1, f.tz2, posArr, normArr, uvArr, idxArr, viRef, 0.4, 1.0);
}

// ============================ CHUNK MESHING ============================
function rebuildChunk(cx, cz) {
  const key = cx + ',' + cz;
  const existing = chunks.get(key);
  if (existing) {
    if (existing.solid) { scene.remove(existing.solid); existing.solid.geometry.dispose(); }
    if (existing.leaf) { scene.remove(existing.leaf); existing.leaf.geometry.dispose(); }
    if (existing.glass) { scene.remove(existing.glass); existing.glass.geometry.dispose(); }
    if (existing.tnt) { scene.remove(existing.tnt); existing.tnt.geometry.dispose(); }
    if (existing.torch) { scene.remove(existing.torch); existing.torch.geometry.dispose(); }
    if (existing.water) { scene.remove(existing.water); existing.water.geometry.dispose(); }
  }

  const solidPos = [], solidNorm = [], solidUV = [], solidIdx = [];
  const leafPos = [], leafNorm = [], leafUV = [], leafIdx = [];
  const glassPos = [], glassNorm = [], glassUV = [], glassIdx = [];
  const tntPos = [], tntNorm = [], tntUV = [], tntIdx = [];
  const torchPos = [], torchNorm = [], torchUV = [], torchIdx = [];
  const waterPos = [], waterNorm = [], waterUV = [], waterIdx = [];
  let sVI = 0, lVI = 0, gVI = 0, tVI = 0, torchVI = {vi: 0}, wVI = 0;

  const x0 = cx * CHUNK_SIZE, z0 = cz * CHUNK_SIZE;
  for (let lx = 0; lx < CHUNK_SIZE; lx++) {
    for (let lz = 0; lz < CHUNK_SIZE; lz++) {
      for (let y = 0; y < WORLD_HEIGHT; y++) {
        const x = x0 + lx, z = z0 + lz;
        const bt = getBlock(x, y, z);
        if (bt === AIR) continue;
        const props = BLOCK_PROPS[bt];
        if (!props && !isTorch(bt)) continue;

        const isWaterBlock = isWater(bt);
        let posArr, normArr, uvArr, idxArr, vi;
        
        if (isWaterBlock) {
          posArr = waterPos; normArr = waterNorm; uvArr = waterUV; idxArr = waterIdx; vi = wVI;
        } else if (bt === LEAVES) {
          posArr = leafPos; normArr = leafNorm; uvArr = leafUV; idxArr = leafIdx; vi = lVI;
        } else if (bt === GLASS) {
          posArr = glassPos; normArr = glassNorm; uvArr = glassUV; idxArr = glassIdx; vi = gVI;
        } else if (bt === TNT) {
          posArr = tntPos; normArr = tntNorm; uvArr = tntUV; idxArr = tntIdx; vi = tVI;
        } else if (isTorch(bt)) {
          const dir = bt === TORCH ? 'UP' : (bt === TORCH_N ? 'NORTH' : (bt === TORCH_S ? 'SOUTH' : (bt === TORCH_E ? 'EAST' : 'WEST')));
          addTorch(x, y, z, dir, torchPos, torchNorm, torchUV, torchIdx, torchVI);
          continue;
        } else {
          posArr = solidPos; normArr = solidNorm; uvArr = solidUV; idxArr = solidIdx; vi = sVI;
        }
        
        if (isWaterBlock) {
          let myHeight = getWaterHeight(bt);
          if (getBlock(x, y - 1, z) === AIR) myHeight = 1.0; 
          
          const tile = TILES[props.top];
          const tx = tile[0], ty = tile[1];
          const u0 = tx / ATLAS_N + 0.001 / (ATLAS_N * TILE_PX);
          const u1 = (tx + 1) / ATLAS_N - 0.001 / (ATLAS_N * TILE_PX);
          const v0 = 1 - (ty + 1) / ATLAS_N + 0.001 / (ATLAS_N * TILE_PX);
          const v1 = 1 - ty / ATLAS_N - 0.001 / (ATLAS_N * TILE_PX);

          const above = getBlock(x, y + 1, z);
          if (!isWater(above)) {
            const topY = y + myHeight;
            posArr.push(x, topY, z+1,  x+1, topY, z+1,  x+1, topY, z,  x, topY, z);
            normArr.push(0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0);
            uvArr.push(u0, v0, u1, v0, u1, v1, u0, v1);
            idxArr.push(vi, vi+1, vi+2, vi, vi+2, vi+3);
            vi += 4;
          }

          const below = getBlock(x, y - 1, z);
          if (below === AIR) {
            posArr.push(x, y, z,  x+1, y, z,  x+1, y, z+1,  x, y, z+1);
            normArr.push(0, -1, 0, 0, -1, 0, 0, -1, 0, 0, -1, 0);
            uvArr.push(u0, v1, u1, v1, u1, v0, u0, v0);
            idxArr.push(vi, vi+1, vi+2, vi, vi+2, vi+3);
            vi += 4;
          }

          const sideTile = TILES[props.side];
          const stx = sideTile[0], sty = sideTile[1];
          const su0 = stx / ATLAS_N + 0.001 / (ATLAS_N * TILE_PX);
          const su1 = (stx + 1) / ATLAS_N - 0.001 / (ATLAS_N * TILE_PX);
          const sv0 = 1 - (sty + 1) / ATLAS_N + 0.001 / (ATLAS_N * TILE_PX);
          const sv1 = 1 - sty / ATLAS_N - 0.001 / (ATLAS_N * TILE_PX);

          const sides = [
            { dir: [1, 0, 0], corners: [[1, 0, 1], [1, 0, 0], [1, 1, 0], [1, 1, 1]] },
            { dir: [-1, 0, 0], corners: [[0, 0, 0], [0, 0, 1], [0, 1, 1], [0, 1, 0]] },
            { dir: [0, 0, 1], corners: [[0, 0, 1], [1, 0, 1], [1, 1, 1], [0, 1, 1]] },
            { dir: [0, 0, -1], corners: [[1, 0, 0], [0, 0, 0], [0, 1, 0], [1, 1, 0]] }
          ];

          for (let s = 0; s < 4; s++) {
            const side = sides[s];
            const nb = getBlock(x + side.dir[0], y, z + side.dir[2]);
            if (isSolid(nb)) continue;
            
            let topY = y + myHeight;
            let botY = y;
            
            if (isWater(nb)) {
              let nbHeight = getWaterHeight(nb);
              if (getBlock(x + side.dir[0], y - 1, z + side.dir[2]) === AIR) nbHeight = 1.0; 
              if (Math.abs(myHeight - nbHeight) < 0.01) continue;
              if (myHeight < nbHeight) continue;
              topY = y + myHeight;
              botY = y + nbHeight;
            } else {
              const aboveBlock = getBlock(x, y + 1, z);
              if (isWater(aboveBlock)) topY = y + 1.0;
              const belowBlock = getBlock(x, y - 1, z);
              if (isWater(belowBlock)) {
                let belowHeight = getWaterHeight(belowBlock);
                if (getBlock(x, y - 2, z) === AIR) belowHeight = 1.0;
                botY = (y - 1) + belowHeight;
              }
            }
            
            for (let c = 0; c < 4; c++) {
              const cx2 = side.corners[c][0];
              const cy2 = side.corners[c][1];
              const cz2 = side.corners[c][2];
              const yPos = cy2 === 1 ? topY : botY;
              posArr.push(x + cx2, yPos, z + cz2);
              normArr.push(side.dir[0], 0, side.dir[2]);
              
              let uCoord;
              if (side.dir[0] !== 0) uCoord = (cz2 === 1) ? su1 : su0;
              else uCoord = (cx2 === 1) ? su1 : su0;
              
              const vCoord = (cy2 === 1) ? sv1 : sv0;
              uvArr.push(uCoord, vCoord);
            }
            idxArr.push(vi, vi+1, vi+2, vi, vi+2, vi+3);
            vi += 4;
          }
          wVI = vi;
        } else {
          for (let f = 0; f < 6; f++) {
            const face = FACES[f];
            const nbx = x + face.dir[0], nby = y + face.dir[1], nbz = z + face.dir[2];
            const nb = getBlock(nbx, nby, nbz);
            
            if (nb !== AIR) {
              if (isWater(nb)) { /* render solid face against water */ }
              else if (isTransparent(nb) && nb === bt) continue;
              else if (isTransparent(nb)) { /* render */ }
              else continue;
            }

            let tileName;
            if (f === 2) tileName = props.top;
            else if (f === 3) tileName = props.bottom;
            else tileName = props.side;
            const tile = TILES[tileName];

            const tx = tile[0], ty = tile[1];
            const u0 = tx / ATLAS_N + 0.001 / (ATLAS_N * TILE_PX);
            const u1 = (tx + 1) / ATLAS_N - 0.001 / (ATLAS_N * TILE_PX);
            const v0 = 1 - (ty + 1) / ATLAS_N + 0.001 / (ATLAS_N * TILE_PX);
            const v1 = 1 - ty / ATLAS_N - 0.001 / (ATLAS_N * TILE_PX);

            for (let c = 0; c < 4; c++) {
              posArr.push(x + face.corners[c][0], y + face.corners[c][1], z + face.corners[c][2]);
              normArr.push(face.dir[0], face.dir[1], face.dir[2]);
              const wu = face.uvs[c][0], wv = face.uvs[c][1];
              uvArr.push(u0 + wu * (u1 - u0), v0 + wv * (v1 - v0));
            }
            idxArr.push(vi, vi+1, vi+2, vi, vi+2, vi+3);
            vi += 4;
          }
          if (bt === LEAVES) lVI = vi;
          else if (bt === GLASS) gVI = vi;
          else if (bt === TNT) tVI = vi;
          else sVI = vi;
        }
      }
    }
  }

  const chunkObj = {};
  if (solidPos.length > 0) {
    const geo = new THREE.BufferGeometry();
    geo.setAttribute('position', new THREE.Float32BufferAttribute(solidPos, 3));
    geo.setAttribute('normal', new THREE.Float32BufferAttribute(solidNorm, 3));
    geo.setAttribute('uv', new THREE.Float32BufferAttribute(solidUV, 2));
    geo.setIndex(solidIdx);
    chunkObj.solid = new THREE.Mesh(geo, solidMat);
    scene.add(chunkObj.solid);
  }
  if (leafPos.length > 0) {
    const geo = new THREE.BufferGeometry();
    geo.setAttribute('position', new THREE.Float32BufferAttribute(leafPos, 3));
    geo.setAttribute('normal', new THREE.Float32BufferAttribute(leafNorm, 3));
    geo.setAttribute('uv', new THREE.Float32BufferAttribute(leafUV, 2));
    geo.setIndex(leafIdx);
    chunkObj.leaf = new THREE.Mesh(geo, leafMat);
    scene.add(chunkObj.leaf);
  }
  if (glassPos.length > 0) {
    const geo = new THREE.BufferGeometry();
    geo.setAttribute('position', new THREE.Float32BufferAttribute(glassPos, 3));
    geo.setAttribute('normal', new THREE.Float32BufferAttribute(glassNorm, 3));
    geo.setAttribute('uv', new THREE.Float32BufferAttribute(glassUV, 2));
    geo.setIndex(glassIdx);
    chunkObj.glass = new THREE.Mesh(geo, glassMat);
    scene.add(chunkObj.glass);
  }
  if (tntPos.length > 0) {
    const geo = new THREE.BufferGeometry();
    geo.setAttribute('position', new THREE.Float32BufferAttribute(tntPos, 3));
    geo.setAttribute('normal', new THREE.Float32BufferAttribute(tntNorm, 3));
    geo.setAttribute('uv', new THREE.Float32BufferAttribute(tntUV, 2));
    geo.setIndex(tntIdx);
    chunkObj.tnt = new THREE.Mesh(geo, tntMat);
    scene.add(chunkObj.tnt);
  }
  if (torchPos.length > 0) {
    const geo = new THREE.BufferGeometry();
    geo.setAttribute('position', new THREE.Float32BufferAttribute(torchPos, 3));
    geo.setAttribute('normal', new THREE.Float32BufferAttribute(torchNorm, 3));
    geo.setAttribute('uv', new THREE.Float32BufferAttribute(torchUV, 2));
    geo.setIndex(torchIdx);
    chunkObj.torch = new THREE.Mesh(geo, torchMat);
    scene.add(chunkObj.torch);
  }
  if (waterPos.length > 0) {
    const geo = new THREE.BufferGeometry();
    geo.setAttribute('position', new THREE.Float32BufferAttribute(waterPos, 3));
    geo.setAttribute('normal', new THREE.Float32BufferAttribute(waterNorm, 3));
    geo.setAttribute('uv', new THREE.Float32BufferAttribute(waterUV, 2));
    geo.setIndex(waterIdx);
    chunkObj.water = new THREE.Mesh(geo, waterMat);
    scene.add(chunkObj.water);
  }
  chunks.set(key, chunkObj);
}

// ============================ HELD ITEM ============================
function updateHeldItem() {
  if (heldMesh) { heldGroup.remove(heldMesh); heldMesh.geometry.dispose(); heldMesh.material.dispose(); heldMesh = null; }
  const bt = HOTBAR[selectedSlot];
  
  if (bt === PISTOL) {
    const geo = new THREE.BoxGeometry(0.1, 0.15, 0.4);
    const mat = new THREE.MeshLambertMaterial({ color: 0x333333 });
    heldMesh = new THREE.Mesh(geo, mat);
    heldMesh.position.set(0.1, -0.1, -0.2);
    heldGroup.add(heldMesh);
    return;
  }
  if (bt === BAZOOKA) {
    const geo = new THREE.CylinderGeometry(0.15, 0.15, 0.8, 8);
    const mat = new THREE.MeshLambertMaterial({ color: 0x2a4a2a });
    heldMesh = new THREE.Mesh(geo, mat);
    heldMesh.rotation.x = Math.PI / 2;
    heldMesh.position.set(0.2, -0.15, -0.3);
    heldGroup.add(heldMesh);
    return;
  }

  const props = BLOCK_PROPS[bt];
  if (!props && !isTorch(bt)) return;
  const geo = new THREE.BoxGeometry(0.32, 0.32, 0.32);
  const uvs = geo.attributes.uv;
  
  if (isTorch(bt)) {
    // Use the same shader logic for the held torch
    let mat = torchMat.clone();
    mat.onBeforeCompile = (shader) => {
      shader.uniforms.time = { value: 0 };
      shader.vertexShader = 'varying vec2 vTorchUv;\n' + shader.vertexShader;
      shader.vertexShader = shader.vertexShader.replace(
        '#include <uv_vertex>',
        `#include <uv_vertex>
         vTorchUv = uv;`
      );
      shader.fragmentShader = 'varying vec2 vTorchUv;\nuniform float time;\n' + shader.fragmentShader;
      shader.fragmentShader = shader.fragmentShader.replace(
        '#include <color_fragment>',
        `
        #include <color_fragment>
        if (vTorchUv.y > 0.4) {
            float t = time * 6.0 + vTorchUv.x * 10.0 + vTorchUv.y * 5.0;
            float flicker = sin(t) * 0.5 + 0.5;
            float fireHeight = (vTorchUv.y - 0.4) / 0.6;
            vec3 color1 = vec3(1.0, 0.2, 0.0);
            vec3 color2 = vec3(1.0, 0.8, 0.0);
            float mixFactor = clamp(fireHeight + flicker * 0.3, 0.0, 1.0);
            vec3 fireColor = mix(color2, color1, mixFactor);
            diffuseColor.rgb = fireColor;
        } else {
            diffuseColor.rgb = vec3(0.20, 0.12, 0.05);
        }
        `
      );
      mat.userData.shader = shader;
    };
    heldMesh = new THREE.Mesh(geo, mat);
  } else {
    const faceTiles = [props.side, props.side, props.top, props.bottom, props.side, props.side];
    for (let f = 0; f < 6; f++) {
      const tile = TILES[faceTiles[f]];
      const tx = tile[0], ty = tile[1];
      const u0 = tx / ATLAS_N, u1 = (tx+1)/ATLAS_N;
      const v0 = 1 - (ty+1)/ATLAS_N, v1 = 1 - ty/ATLAS_N;
      const b = f * 4;
      uvs.setXY(b+0, u0, v1);
      uvs.setXY(b+1, u1, v1);
      uvs.setXY(b+2, u0, v0);
      uvs.setXY(b+3, u1, v0);
    }
    uvs.needsUpdate = true;
    let mat = solidMat;
    if (bt === TNT) mat = tntMat;
    if (bt === LEAVES) mat = leafMat;
    if (bt === GLASS) mat = glassMat;
    if (isWater(bt)) mat = waterMat;
    heldMesh = new THREE.Mesh(geo, mat);
  }
  heldGroup.add(heldMesh);
}

// ============================ INPUT ============================
function setupInput() {
  addEventListener('keydown', e => {
    keys[e.code] = true;
    if (e.code === 'Space' && gameState === 'PLAYING') e.preventDefault();
    if (gameState !== 'PLAYING' && !isInventoryOpen) return;

    if (e.code.startsWith('Digit')) {
      const n = parseInt(e.code.slice(5));
      if (n >= 1 && n <= HOTBAR.length) selectSlot(n - 1);
    }
    if (e.code === 'KeyE') {
      if (gameStarted) {
        if (isInventoryOpen) closeInventory();
        else openInventory();
      }
    }
    if (e.code === 'Escape' && isInventoryOpen) closeInventory();
  });
  addEventListener('keyup', e => { keys[e.code] = false; });

  addEventListener('mousedown', e => {
    if (!controls.isLocked) return;
    if (e.button === 0) { mouseLeft = true; tryActionLeftClick(); }
    if (e.button === 2) { mouseRight = true; tryPlaceBlock(); placeCD = 0.22; }
  });
  addEventListener('mouseup', e => {
    if (e.button === 0) mouseLeft = false;
    if (e.button === 2) mouseRight = false;
  });
  addEventListener('contextmenu', e => e.preventDefault());

  addEventListener('wheel', e => {
    if (!controls.isLocked) return;
    if (e.deltaY > 0) selectSlot((selectedSlot + 1) % HOTBAR.length);
    else selectSlot((selectedSlot - 1 + HOTBAR.length) % HOTBAR.length);
  });
}

// ============================ HOTBAR ============================
function setupHotbar() {
  const hb = document.getElementById('hotbar');
  hb.innerHTML = '';
  HOTBAR.forEach((bt, i) => {
    const slot = document.createElement('div');
    slot.className = 'slot' + (i === selectedSlot ? ' selected' : '');
    slot.innerHTML = `<span class="slot-num">${i+1}</span>`;
    const cvs = document.createElement('canvas');
    cvs.width = 42; cvs.height = 42;
    slot.appendChild(cvs);
    hb.appendChild(slot);
    drawBlockIcon(cvs, bt);
    slot.addEventListener('click', () => selectSlot(i));
  });
}
function selectSlot(i) {
  selectedSlot = i;
  document.querySelectorAll('#hotbar .slot').forEach((s, idx) => s.classList.toggle('selected', idx === i));
  updateHeldItem();
  showToast(BLOCK_NAMES[HOTBAR[i]]);
}
function drawBlockIcon(canvas, bt) {
  const ctx = canvas.getContext('2d');
  ctx.imageSmoothingEnabled = false;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  
  if (bt === PISTOL) {
    ctx.fillStyle = '#333';
    ctx.fillRect(15, 15, 12, 8); 
    ctx.fillRect(18, 23, 6, 8);  
    ctx.fillStyle = '#555';
    ctx.fillRect(15, 15, 12, 2);
    return;
  }
  if (bt === BAZOOKA) {
    ctx.fillStyle = '#2a4a2a';
    ctx.fillRect(10, 18, 24, 8);
    ctx.fillStyle = '#444';
    ctx.fillRect(10, 18, 6, 8); 
    ctx.fillStyle = '#111';
    ctx.fillRect(30, 19, 4, 6); 
    return;
  }

  const props = BLOCK_PROPS[bt];
  if (!props && !isTorch(bt)) return;
  
  if (isTorch(bt)) {
    // Draw custom torch icon
    ctx.fillStyle = '#332008'; // Dark browner stick
    ctx.fillRect(19, 24, 4, 10);
    ctx.fillStyle = '#FF4500'; // Red fire
    ctx.fillRect(15, 14, 12, 10);
    ctx.fillStyle = '#FFD700'; // Yellow fire
    ctx.fillRect(18, 12, 6, 4);
    return;
  }

  const topTile = TILES[props.top], sideTile = TILES[props.side];
  const s = canvas.width;
  ctx.drawImage(atlasCanvas, topTile[0]*TILE_PX, topTile[1]*TILE_PX, TILE_PX, TILE_PX, 0, 0, s, s/2);
  ctx.drawImage(atlasCanvas, sideTile[0]*TILE_PX, sideTile[1]*TILE_PX, TILE_PX, TILE_PX, 0, s/2, s, s/2);
  ctx.fillStyle = 'rgba(0,0,0,0.15)';
  ctx.fillRect(0, s/2, s, s/2);
}

// ============================ INVENTORY ============================
function setupInventory() {
  document.querySelectorAll('.inv-tab').forEach(tab => {
    tab.addEventListener('click', () => {
      currentTab = tab.dataset.tab;
      document.querySelectorAll('.inv-tab').forEach(t => t.classList.remove('active'));
      tab.classList.add('active');
      renderInventoryGrid();
    });
  });
  document.getElementById('invCloseBtn').addEventListener('click', closeInventory);
  
  const invContainer = document.getElementById('inventory');
  invContainer.addEventListener('mousemove', (e) => {
    const cursorItemDiv = document.getElementById('cursor-item');
    cursorItemDiv.style.left = e.clientX + 'px';
    cursorItemDiv.style.top = e.clientY + 'px';
  });

  renderInventoryGrid();
  renderInvHotbar();
}

function updateCursorItem() {
  const cursorItemDiv = document.getElementById('cursor-item');
  const cvs = cursorItemDiv.querySelector('canvas');
  if (cursorItem === AIR) {
    cursorItemDiv.style.display = 'none';
  } else {
    cursorItemDiv.style.display = 'block';
    drawBlockIcon(cvs, cursorItem);
  }
}

function handleSlotClick(type, index) {
  if (type === 'creative') {
    cursorItem = INVENTORY_CATS[currentTab][index];
  } else if (type === 'player') {
    const temp = playerInv[index];
    playerInv[index] = cursorItem;
    cursorItem = temp;
    renderInventoryGrid();
  } else if (type === 'hotbar') {
    const temp = HOTBAR[index];
    HOTBAR[index] = cursorItem;
    cursorItem = temp;
    renderInvHotbar();
    setupHotbar();
    updateHeldItem();
  }
  updateCursorItem();
}

function renderInventoryGrid() {
  const grid = document.getElementById('invGrid');
  grid.innerHTML = '';
  
  if (currentTab === 'player') {
    // Render Player Inventory (27 slots)
    for (let i = 0; i < 27; i++) {
      const bt = playerInv[i];
      const item = document.createElement('div');
      item.className = 'inv-item';
      const cvs = document.createElement('canvas');
      cvs.width = 48; cvs.height = 48;
      item.appendChild(cvs);
      if (bt !== AIR) drawBlockIcon(cvs, bt);
      item.addEventListener('click', () => handleSlotClick('player', i));
      grid.appendChild(item);
    }
  } else {
    // Render Creative Tab
    const blocks = INVENTORY_CATS[currentTab];
    blocks.forEach((bt, i) => {
      const item = document.createElement('div');
      item.className = 'inv-item' + (selectedInvBlock === bt ? ' selected' : '');
      item.title = BLOCK_NAMES[bt];
      const cvs = document.createElement('canvas');
      cvs.width = 48; cvs.height = 48;
      item.appendChild(cvs);
      drawBlockIcon(cvs, bt);
      item.addEventListener('click', () => handleSlotClick('creative', i));
      grid.appendChild(item);
    });
  }
}

function renderInvHotbar() {
  const hb = document.getElementById('invHotbar');
  hb.innerHTML = '';
  HOTBAR.forEach((bt, i) => {
    const slot = document.createElement('div');
    slot.className = 'inv-hb-slot';
    slot.innerHTML = `<span class="num">${i+1}</span>`;
    const cvs = document.createElement('canvas');
    cvs.width = 42; cvs.height = 42;
    slot.appendChild(cvs);
    if (bt !== AIR) drawBlockIcon(cvs, bt);
    slot.addEventListener('click', () => handleSlotClick('hotbar', i));
    hb.appendChild(slot);
  });
}

function openInventory() {
  isInventoryOpen = true;
  document.getElementById('inventory').classList.add('show');
  controls.unlock();
  document.getElementById('crosshair').style.display = 'none';
}
function closeInventory() {
  isInventoryOpen = false;
  document.getElementById('inventory').classList.remove('show');
  cursorItem = AIR; // Drop cursor item
  updateCursorItem();
  controls.lock();
}

// ============================ PARTICLES & DEBRIS ============================
function getBlockColor(bt) {
  const props = BLOCK_PROPS[bt];
  if (!props) return 0xffffff;
  const tile = TILES[props.side];
  const tx = tile[0] * TILE_PX, ty = tile[1] * TILE_PX;
  const tmpCvs = document.createElement('canvas');
  tmpCvs.width = TILE_PX; tmpCvs.height = TILE_PX;
  tmpCvs.getContext('2d').drawImage(atlasCanvas, tx, ty, TILE_PX, TILE_PX, 0, 0, TILE_PX, TILE_PX);
  const data = tmpCvs.getContext('2d').getImageData(4, 4, 1, 1).data;
  return (data[0] << 16) | (data[1] << 8) | data[2];
}
function spawnParticles(x, y, z, bt, count = 8) {
  const color = getBlockColor(bt);
  for (let i = 0; i < count; i++) {
    const size = 0.08 + Math.random() * 0.06;
    const geo = new THREE.BoxGeometry(size, size, size);
    const mat = new THREE.MeshLambertMaterial({ color: color });
    const m = new THREE.Mesh(geo, mat);
    m.position.set(x + 0.5, y + 0.5, z + 0.5);
    m.userData = {
      vel: new THREE.Vector3((Math.random()-0.5)*4, Math.random()*4+2, (Math.random()-0.5)*4),
      life: 0.8 + Math.random() * 0.4,
      maxLife: 1.2,
      isDebris: false,
      isTorchParticle: false,
      size: size
    };
    scene.add(m);
    particles.push(m);
  }
}
function spawnTorchParticle(x, y, z) {
  const isRed = Math.random() > 0.5;
  const mat = isRed ? torchMatRed.clone() : torchMatYellow.clone();
  const m = new THREE.Mesh(torchParticleGeo, mat);
  m.position.set(x + 0.5, y + 0.8, z + 0.5);
  m.userData = {
    vel: new THREE.Vector3((Math.random()-0.5)*0.5, Math.random()*1.5 + 0.5, (Math.random()-0.5)*0.5),
    life: 0.5 + Math.random() * 0.5,
    maxLife: 1.0,
    isDebris: false,
    isTorchParticle: true,
    size: 0.06
  };
  scene.add(m);
  particles.push(m);
}
function spawnDebris(x, y, z, bt, dir, force) {
  const color = getBlockColor(bt);
  const size = 0.2 + Math.random() * 0.1;
  const geo = new THREE.BoxGeometry(size, size, size);
  const mat = new THREE.MeshLambertMaterial({ color: color });
  const m = new THREE.Mesh(geo, mat);
  m.position.set(x + 0.5, y + 0.5, z + 0.5);
  m.userData = {
    vel: new THREE.Vector3(
      dir.x * force + (Math.random()-0.5)*2, 
      dir.y * force + Math.random()*3 + 2, 
      dir.z * force + (Math.random()-0.5)*2
    ),
    life: 3.0 + Math.random() * 2.0,
    maxLife: 5.0,
    isDebris: true,
    isTorchParticle: false,
    size: size
  };
  scene.add(m);
  particles.push(m);
}
function updateParticles(dt) {
  for (let i = particles.length - 1; i >= 0; i--) {
    const p = particles[i];
    
    if (p.userData.isTorchParticle) {
      p.userData.vel.y += 2 * dt; // Floats up
      p.material.opacity = p.userData.life / p.userData.maxLife;
    } else {
      p.userData.vel.y -= 16 * dt;
      if (p.userData.isDebris) {
        const pY = p.position.y - p.userData.size / 2;
        if (p.userData.vel.y < 0 && isSolid(getBlock(p.position.x, pY - 0.05, p.position.z))) {
          p.position.y = Math.floor(pY) + 1 + p.userData.size / 2;
          p.userData.vel.y = -p.userData.vel.y * 0.35;
          p.userData.vel.x *= 0.6;
          p.userData.vel.z *= 0.6;
        }
      }
    }
    
    p.position.x += p.userData.vel.x * dt;
    p.position.y += p.userData.vel.y * dt;
    p.position.z += p.userData.vel.z * dt;
    p.userData.life -= dt;
    
    if (!p.userData.isTorchParticle) {
      p.rotation.x += dt * 5; p.rotation.z += dt * 3;
    }
    
    if (p.userData.life <= 0 || p.position.y < -10) {
      scene.remove(p); p.geometry.dispose(); p.material.dispose();
      particles.splice(i, 1);
    }
  }
}

// ============================ RAYCAST ============================
function raycastVoxel(maxDist = REACH) {
  const origin = camera.position;
  const dir = new THREE.Vector3();
  camera.getWorldDirection(dir);
  let x = Math.floor(origin.x), y = Math.floor(origin.y), z = Math.floor(origin.z);
  const stepX = Math.sign(dir.x), stepY = Math.sign(dir.y), stepZ = Math.sign(dir.z);
  const tDX = stepX !== 0 ? Math.abs(1 / dir.x) : Infinity;
  const tDY = stepY !== 0 ? Math.abs(1 / dir.y) : Infinity;
  const tDZ = stepZ !== 0 ? Math.abs(1 / dir.z) : Infinity;
  let tMX = stepX > 0 ? (Math.ceil(origin.x) - origin.x) * tDX : (origin.x - Math.floor(origin.x)) * tDX;
  let tMY = stepY > 0 ? (Math.ceil(origin.y) - origin.y) * tDY : (origin.y - Math.floor(origin.y)) * tDY;
  let tMZ = stepZ > 0 ? (Math.ceil(origin.z) - origin.z) * tDZ : (origin.z - Math.floor(origin.z)) * tDZ;
  if (stepX === 0) tMX = Infinity;
  if (stepY === 0) tMY = Infinity;
  if (stepZ === 0) tMZ = Infinity;

  let face = [0, 0, 0];
  for (let i = 0; i < 80; i++) {
    const b = getBlock(x, y, z);
    if (b !== AIR && !isWater(b)) return { x, y, z, face, block: b };
    if (tMX < tMY && tMX < tMZ) { x += stepX; if (tMX > maxDist) break; tMX += tDX; face = [-stepX, 0, 0]; }
    else if (tMY < tMZ) { y += stepY; if (tMY > maxDist) break; tMY += tDY; face = [0, -stepY, 0]; }
    else { z += stepZ; if (tMZ > maxDist) break; tMZ += tDZ; face = [0, 0, -stepZ]; }
  }
  return null;
}

// ============================ ACTIONS (BREAK / PLACE / SHOOT) ============================
function tryActionLeftClick() {
  const item = HOTBAR[selectedSlot];
  if (item === PISTOL) { tryShootPistol(); return; }
  if (item === BAZOOKA) { tryShootBazooka(); return; }
  
  if (breakCD <= 0) {
    tryBreakBlock();
    breakCD = 0.22;
  }
}

function tryBreakBlock() {
  const hit = raycastVoxel();
  if (!hit) return;
  if (hit.block === TNT) {
    explodeTNT(hit.x, hit.y, hit.z);
    return;
  }
  spawnParticles(hit.x, hit.y, hit.z, hit.block, 8);
  setBlock(hit.x, hit.y, hit.z, AIR);
  playSound('break');
  swingTime = 0.3;
}

function tryPlaceBlock() {
  const item = HOTBAR[selectedSlot];
  if (item === PISTOL || item === BAZOOKA) return; 

  const hit = raycastVoxel();
  if (!hit) return;
  const nx = hit.x + hit.face[0], ny = hit.y + hit.face[1], nz = hit.z + hit.face[2];
  if (nx < 0 || nx >= WORLD_SIZE || ny < 0 || ny >= WORLD_HEIGHT || nz < 0 || nz >= WORLD_SIZE) return;
  if (getBlock(nx, ny, nz) !== AIR && !isWater(getBlock(nx, ny, nz))) return;
  
  let bt = item;
  // Torch Placement Logic
  if (item === TORCH) {
    if (hit.face[0] === 1) bt = TORCH_E;
    else if (hit.face[0] === -1) bt = TORCH_W;
    else if (hit.face[2] === 1) bt = TORCH_S;
    else if (hit.face[2] === -1) bt = TORCH_N;
    else {
      const below = getBlock(nx, ny - 1, nz);
      if (!isSolid(below)) return; // Must be on solid ground if placed on floor
    }
  }
  
  const pMinX = player.pos.x - PLAYER_W/2, pMaxX = player.pos.x + PLAYER_W/2;
  const pMinY = player.pos.y, pMaxY = player.pos.y + PLAYER_H;
  const pMinZ = player.pos.z - PLAYER_W/2, pMaxZ = player.pos.z + PLAYER_W/2;
  if (nx+1 > pMinX && nx < pMaxX && ny+1 > pMinY && ny < pMaxY && nz+1 > pMinZ && nz < pMaxZ) return;
  
  setBlock(nx, ny, nz, bt);
  if (bt === TNT) {
    activeTNT.push({ x: nx, y: ny, z: nz, time: 5.0 });
  }
  playSound('place');
  swingTime = 0.3;
}

function tryShootPistol() {
  if (breakCD > 0) return;
  breakCD = 0.3;
  playSound('shoot');
  swingTime = 0.2;
  
  const hit = raycastVoxel(50); 
  if (hit) {
    spawnParticles(hit.x, hit.y, hit.z, hit.block, 12);
    setBlock(hit.x, hit.y, hit.z, AIR);
    playSound('break');
  }
}

function tryShootBazooka() {
  if (breakCD > 0) return;
  breakCD = 1.5;
  playSound('shoot');
  swingTime = 0.5;
  
  const projGeo = new THREE.BoxGeometry(0.4, 0.4, 0.4);
  const projMat = new THREE.MeshLambertMaterial({ color: 0xaa3030, emissive: 0xaa3030, emissiveIntensity: 0.5 });
  const projMesh = new THREE.Mesh(projGeo, projMat);
  projMesh.position.copy(camera.position);
  
  const dir = new THREE.Vector3();
  camera.getWorldDirection(dir);
  projMesh.userData = {
    vel: dir.multiplyScalar(30),
    life: 5.0
  };
  scene.add(projMesh);
  projectiles.push(projMesh);
}

function updateProjectiles(dt) {
  for (let i = projectiles.length - 1; i >= 0; i--) {
    const p = projectiles[i];
    p.userData.vel.y -= 20 * dt; 
    p.position.x += p.userData.vel.x * dt;
    p.position.y += p.userData.vel.y * dt;
    p.position.z += p.userData.vel.z * dt;
    p.userData.life -= dt;
    
    const bx = Math.floor(p.position.x);
    const by = Math.floor(p.position.y);
    const bz = Math.floor(p.position.z);
    
    if (isSolid(getBlock(bx, by, bz))) {
      explodeTNT(bx, by, bz);
      scene.remove(p);
      p.geometry.dispose();
      p.material.dispose();
      projectiles.splice(i, 1);
      continue;
    }
    
    if (p.userData.life <= 0 || p.position.y < -10) {
      scene.remove(p);
      p.geometry.dispose();
      p.material.dispose();
      projectiles.splice(i, 1);
    }
  }
}

function explodeTNT(x, y, z) {
  const R = 3.5, R2 = R * R;
  setBlock(x, y, z, AIR);
  for (let dx = -Math.ceil(R); dx <= Math.ceil(R); dx++) {
    for (let dy = -Math.ceil(R); dy <= Math.ceil(R); dy++) {
      for (let dz = -Math.ceil(R); dz <= Math.ceil(R); dz++) {
        const d2 = dx*dx + dy*dy + dz*dz;
        if (d2 > R2) continue;
        const bx = x + dx, by = y + dy, bz = z + dz;
        const b = getBlock(bx, by, bz);
        if (b === AIR || isWater(b)) continue;
        
        if (b === TNT) {
          activeTNT.push({ x: bx, y: by, z: bz, time: 0.3 });
          continue;
        }
        
        const dist = Math.sqrt(d2);
        const force = (R - dist) * 2.5 + 2;
        const dir = new THREE.Vector3(dx, dy, dz).normalize();
        spawnDebris(bx, by, bz, b, dir, force);
        setBlock(bx, by, bz, AIR);
      }
    }
  }
  playSound('explosion');
  shakeTime = 0.5; 
  shakeIntensity = 0.5;
}

// ============================ DYNAMIC TORCH LIGHTS ============================
function updateTorchLights(dt) {
  torchScanTimer += dt;
  if (torchScanTimer < 0.5) return; 
  torchScanTimer = 0;

  let nearest = [];
  for (let x = 0; x < WORLD_SIZE; x++) {
    for (let y = 0; y < WORLD_HEIGHT; y++) {
      for (let z = 0; z < WORLD_SIZE; z++) {
        if (isTorch(getBlock(x, y, z))) {
          const dx = x - player.pos.x;
          const dy = y - player.pos.y;
          const dz = z - player.pos.z;
          const distSq = dx*dx + dy*dy + dz*dz;
          nearest.push({distSq, x, y, z});
        }
      }
    }
  }
  
  nearest.sort((a, b) => a.distSq - b.distSq);
  
  for (let i = 0; i < torchLights.length; i++) {
    if (i < nearest.length) {
      const t = nearest[i];
      torchLights[i].light.intensity = 1.5;
      torchLights[i].light.position.set(t.x + 0.5, t.y + 0.8, t.z + 0.5);
      
      // Spawn fire particles
      if (Math.random() < 0.3) {
        spawnTorchParticle(t.x, t.y, t.z);
      }
    } else {
      torchLights[i].light.intensity = 0;
    }
  }
}

// ============================ PLAYER PHYSICS ============================
function checkCollision(pos) {
  const minX = Math.floor(pos.x - PLAYER_W/2), maxX = Math.floor(pos.x + PLAYER_W/2 - 0.001);
  const minY = Math.floor(pos.y), maxY = Math.floor(pos.y + PLAYER_H - 0.001);
  const minZ = Math.floor(pos.z - PLAYER_W/2), maxZ = Math.floor(pos.z + PLAYER_W/2 - 0.001);
  for (let x = minX; x <= maxX; x++)
    for (let y = minY; y <= maxY; y++)
      for (let z = minZ; z <= maxZ; z++)
        if (isSolid(getBlock(x, y, z))) return true;
  return false;
}
function isEyeInWater() { return isWater(getBlock(camera.position.x, camera.position.y, camera.position.z)); }
function isFeetInWater() { return isWater(getBlock(player.pos.x, player.pos.y + 0.2, player.pos.z)); }

function updatePlayer(dt) {
  const eyePos = controls.getObject().position;
  player.pos.x = eyePos.x;
  player.pos.y = eyePos.y - PLAYER_EYE;
  player.pos.z = eyePos.z;

  player.inWater = isFeetInWater() || isWater(getBlock(player.pos.x, player.pos.y + 0.9, player.pos.z));

  const forward = new THREE.Vector3();
  camera.getWorldDirection(forward);
  forward.y = 0; forward.normalize();
  const right = new THREE.Vector3().crossVectors(forward, new THREE.Vector3(0, 1, 0)).normalize();

  let mx = 0, mz = 0;
  if (keys['KeyW']) mz += 1;
  if (keys['KeyS']) mz -= 1;
  if (keys['KeyA']) mx -= 1;
  if (keys['KeyD']) mx += 1;

  const sprint = keys['ShiftLeft'] || keys['ShiftRight'];
  let speed = sprint ? SPRINT : WALK;
  if (player.inWater) speed *= 0.5;

  const wishDir = new THREE.Vector3();
  wishDir.addScaledVector(forward, mz);
  wishDir.addScaledVector(right, mx);
  if (wishDir.lengthSq() > 0) wishDir.normalize();

  player.vel.x = wishDir.x * speed;
  player.vel.z = wishDir.z * speed;

  if (player.inWater) {
    player.vel.y -= GRAVITY * 0.25 * dt;
    player.vel.y *= 0.92;
    if (keys['Space']) player.vel.y = 3.5;
    if (player.vel.y < -6) player.vel.y = -6;
  } else {
    player.vel.y -= GRAVITY * dt;
    if (keys['Space'] && player.onGround) { player.vel.y = JUMP_V; player.onGround = false; }
  }

  const newPos = player.pos.clone();

  newPos.x = player.pos.x + player.vel.x * dt;
  if (!checkCollision(newPos)) player.pos.x = newPos.x;
  else player.vel.x = 0;

  newPos.x = player.pos.x;
  newPos.z = player.pos.z + player.vel.z * dt;
  if (!checkCollision(newPos)) player.pos.z = newPos.z;
  else player.vel.z = 0;

  newPos.z = player.pos.z;
  newPos.y = player.pos.y + player.vel.y * dt;
  if (!checkCollision(newPos)) {
    player.pos.y = newPos.y;
    player.onGround = false;
  } else {
    if (player.vel.y < 0) player.onGround = true;
    player.vel.y = 0;
  }

  if (player.pos.y < -5) {
    const sx = Math.floor(WORLD_SIZE/2), sz = Math.floor(WORLD_SIZE/2);
    for (let y = WORLD_HEIGHT-1; y >= 0; y--) {
      if (isSolid(getBlock(sx, y, sz))) {
        player.pos.set(sx + 0.5, y + 1, sz + 0.5);
        player.vel.set(0, 0, 0);
        break;
      }
    }
  }

  eyePos.x = player.pos.x;
  eyePos.y = player.pos.y + PLAYER_EYE;
  eyePos.z = player.pos.z;

  if (swingTime > 0) {
    swingTime -= dt;
    heldGroup.rotation.z = Math.sin(swingTime * 20) * 0.3 * (swingTime / 0.3);
  } else heldGroup.rotation.z = 0;
  
  const moving = (mx !== 0 || mz !== 0) && player.onGround;
  heldGroup.position.y = -0.45 + (moving ? Math.sin(performance.now() * 0.012) * 0.025 : 0);

  const targetFov = (sprint && (mx !== 0 || mz !== 0) ? settings.fov + 5 : settings.fov);
  camera.fov += (targetFov - camera.fov) * 0.1;
  camera.updateProjectionMatrix();

  document.getElementById('water-overlay').classList.toggle('show', isEyeInWater());
}

// ============================ DAY / NIGHT & SHOOTING STARS ============================
function updateDayNight(dt) {
  dayTime += dt / 120;
  if (dayTime >= 1) dayTime -= 1;
  const angle = dayTime * Math.PI * 2 - Math.PI / 2;
  const sunY = Math.sin(angle), sunX = Math.cos(angle);

  sun.position.set(WORLD_SIZE/2 + sunX * 80, sunY * 80, WORLD_SIZE/2 + 20);
  sun.target.position.set(WORLD_SIZE/2, 0, WORLD_SIZE/2);
  sun.target.updateMatrixWorld();

  const dayFactor = Math.max(0, Math.min(1, (sunY + 0.15) / 0.35));
  sun.intensity = dayFactor * 1.5 + 0.1;
  hemiLight.intensity = 0.3 + dayFactor * 0.6;

  const nightSky = new THREE.Color(0x0a0a25);
  const daySky = new THREE.Color(0x87ceeb);
  const sunsetSky = new THREE.Color(0xff7733);
  const skyColor = nightSky.clone().lerp(daySky, dayFactor);
  const sunsetFactor = Math.max(0, 1 - Math.abs(sunY) / 0.3);
  skyColor.lerp(sunsetSky, sunsetFactor * 0.4 * (1 - Math.abs(sunY * 2)));
  scene.background = skyColor;
  scene.fog.color = skyColor;

  stars.material.opacity = 1 - dayFactor;
  stars.rotation.y += dt * 0.01;

  sunMesh.position.copy(sun.position);
  sunMesh.position.y = Math.max(2, sunMesh.position.y);
  sunMesh.material.color.setRGB(1, 0.9 + dayFactor * 0.1, 0.5 + dayFactor * 0.5);
  
  if (settings.shootingStars && dayFactor < 0.2 && Math.random() < 0.005) {
    const points = [];
    const pX = player.pos.x + (Math.random()-0.5)*100;
    const pZ = player.pos.z + (Math.random()-0.5)*100;
    points.push(new THREE.Vector3(pX, 150 + Math.random()*20, pZ));
    points.push(new THREE.Vector3(pX - 20, 145 + Math.random()*20, pZ - 5));
    const geo = new THREE.BufferGeometry().setFromPoints(points);
    const mat = new THREE.LineBasicMaterial({ color: 0xffffff, transparent: true, opacity: 0.9 });
    const star = new THREE.Line(geo, mat);
    star.userData = { life: 1.0, vel: new THREE.Vector3(-80, -10, -20) };
    scene.add(star);
    shootingStars.push(star);
  }

  for (let i = shootingStars.length - 1; i >= 0; i--) {
    const s = shootingStars[i];
    s.position.addScaledVector(s.userData.vel, dt);
    s.userData.life -= dt * 2;
    s.material.opacity = s.userData.life;
    if (s.userData.life <= 0) {
      scene.remove(s); s.geometry.dispose(); s.material.dispose();
      shootingStars.splice(i, 1);
    }
  }
}

// ============================ UTIL ============================
let toastTimer = null;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 1500);
}
function playSound(type) {
  if (!audioCtx) return;
  const now = audioCtx.currentTime;
  let buf;
  if (type === 'explosion') buf = audioCtx.createBuffer(1, 22050, audioCtx.sampleRate);
  else buf = audioCtx.createBuffer(1, 4410, audioCtx.sampleRate);
  const data = buf.getChannelData(0);
  const decay = type === 'break' ? 1500 : (type === 'explosion' ? 4000 : (type === 'shoot' ? 300 : 2500));
  for (let i = 0; i < data.length; i++) data[i] = (Math.random() * 2 - 1) * Math.exp(-i / decay);
  const src = audioCtx.createBufferSource(); src.buffer = buf;
  const filt = audioCtx.createBiquadFilter();
  filt.type = 'lowpass'; 
  filt.frequency.value = type === 'break' ? 700 : (type === 'explosion' ? 200 : (type === 'shoot' ? 3000 : 1400));
  const gain = audioCtx.createGain(); 
  gain.gain.value = type === 'explosion' ? 0.35 : 0.15;
  src.connect(filt); filt.connect(gain); gain.connect(audioCtx.destination);
  src.start(now);
}
function onResize() {
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth, innerHeight);
  composer.setSize(innerWidth, innerHeight);
}

// ============================ MAIN LOOP ============================
function animate() {
  requestAnimationFrame(animate);
  const now = performance.now();
  let dt = (now - lastTime) / 1000;
  lastTime = now;
  if (dt > 0.1) dt = 0.1;

  if (leafMat && leafMat.userData.shader) {
    leafMat.userData.shader.uniforms.time.value = now / 1000;
  }
  if (torchMat && torchMat.userData.shader) {
    torchMat.userData.shader.uniforms.time.value = now / 1000;
  }
  if (heldMesh && heldMesh.material && heldMesh.material.userData.shader) {
    heldMesh.material.userData.shader.uniforms.time.value = now / 1000;
  }
  if (shaderPass) {
    shaderPass.uniforms.time.value = now / 1000;
  }

  if (gameState === 'LOADING') {
    if (chunksToRebuild.size > 0) {
      const iter = chunksToRebuild.values();
      let count = 0;
      while (count < 8) {
        const next = iter.next();
        if (next.done) break;
        const [cx, cz] = next.value.split(',').map(Number);
        rebuildChunk(cx, cz);
        chunksToRebuild.delete(next.value);
        count++;
      }
      const totalChunks = CHUNKS_PER_SIDE * CHUNKS_PER_SIDE;
      const builtChunks = totalChunks - chunksToRebuild.size;
      const progress = Math.floor((builtChunks / totalChunks) * 100);
      document.getElementById('load-fill').style.width = progress + '%';
      document.getElementById('load-text').textContent = progress + '%';
    } else {
      document.getElementById('load-fill').style.width = '100%';
      document.getElementById('load-text').textContent = '100%';
      document.getElementById('btn-enter-game').style.display = 'block';
    }
  }

  if (gameStarted && gameState === 'PLAYING') {
    if (chunksToRebuild.size > 0) {
      const iter = chunksToRebuild.values();
      let count = 0;
      while (count < 4) {
        const next = iter.next();
        if (next.done) break;
        const [cx, cz] = next.value.split(',').map(Number);
        rebuildChunk(cx, cz);
        chunksToRebuild.delete(next.value);
        count++;
      }
    }

    updatePlayer(dt);

    breakCD -= dt; placeCD -= dt;
    if (mouseLeft && breakCD <= 0) {
      tryActionLeftClick();
    }
    if (mouseRight && placeCD <= 0) { tryPlaceBlock(); placeCD = 0.22; }

    const hit = raycastVoxel();
    if (hit) {
      highlight.visible = true;
      highlight.position.set(hit.x + 0.5, hit.y + 0.5, hit.z + 0.5);
    } else highlight.visible = false;

    for (let i = activeTNT.length - 1; i >= 0; i--) {
      const t = activeTNT[i];
      t.time -= dt;
      if (t.time <= 0) {
        explodeTNT(t.x, t.y, t.z);
        activeTNT.splice(i, 1);
      }
    }

    updateProjectiles(dt);
    updateTorchLights(dt);
    processWaterQueue(dt);
    updateParticles(dt);
    updateDayNight(dt);

    let shakeX = 0, shakeY = 0;
    if (shakeTime > 0) {
      shakeTime -= dt;
      shakeX = (Math.random() - 0.5) * shakeIntensity;
      shakeY = (Math.random() - 0.5) * shakeIntensity;
      camera.position.x += shakeX;
      camera.position.y += shakeY;
    }

    stats.frames++; stats.fpsTime += dt;
    if (stats.fpsTime >= 0.5) {
      stats.fps = Math.round(stats.frames / stats.fpsTime);
      stats.frames = 0; stats.fpsTime = 0;
    }
    document.getElementById('stats').innerHTML = `<span class="label">FPS</span> ${stats.fps}<br><span class="label">XYZ</span> ${player.pos.x.toFixed(1)} ${player.pos.y.toFixed(1)} ${player.pos.z.toFixed(1)}<br><span class="label">BLOCK</span> ${BLOCK_NAMES[HOTBAR[selectedSlot]]}`;

    if (settings.fancyGraphics) {
      composer.render();
    } else {
      renderer.render(scene, camera);
    }
    
    if (shakeTime > 0) {
      camera.position.x -= shakeX;
      camera.position.y -= shakeY;
    }
  } else if (gameState === 'LOADING' || gameState === 'PAUSED' || gameState === 'MENU') {
    if (worldData) {
      if (settings.fancyGraphics) {
        composer.render();
      } else {
        renderer.render(scene, camera);
      }
    }
  }
}
</script>
</body>
</html>
