<!doctype html>
<html><head><meta charset=utf-8><title>Twitch Embed</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="Twitch embed with latency monitoring for Unity WebView">
<style>
html,body{margin:0;height:100%;width:100%;background:#000;font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,Arial,sans-serif;overflow:hidden}
#twitch{position:fixed;inset:0;width:100%;height:100%}
#loading{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);color:#fff;text-align:center;z-index:1000}
.status{position:fixed;top:10px;left:10px;background:rgba(138,43,226,0.95);color:#fff;padding:8px 16px;border-radius:8px;font-size:14px;font-weight:500;z-index:1001;box-shadow:0 4px 12px rgba(0,0,0,0.4);border:1px solid rgba(255,255,255,0.1)}
.success{background:rgba(34,197,94,0.95)!important}
.error{background:rgba(239,68,68,0.95)!important}
.warning{background:rgba(245,158,11,0.95)!important}
.info{background:rgba(59,130,246,0.95)!important}
.spinner{border:3px solid rgba(255,255,255,0.3);border-top:3px solid #fff;border-radius:50%;width:32px;height:32px;animation:spin 1s linear infinite;margin:0 auto 20px}
@keyframes spin{0%{transform:rotate(0deg)}100%{transform:rotate(360deg)}}
.latency-display{position:fixed;top:10px;right:10px;background:rgba(0,0,0,0.8);color:#00ff00;padding:6px 12px;border-radius:6px;font-size:12px;font-family:monospace;z-index:1002;border:1px solid rgba(0,255,0,0.3);display:none}
.controls{position:fixed;bottom:10px;left:10px;display:flex;gap:8px;z-index:1003}
.btn{background:rgba(0,0,0,0.8);color:#fff;border:1px solid rgba(255,255,255,0.3);padding:6px 12px;border-radius:6px;font-size:12px;cursor:pointer;transition:all 0.2s}
.btn:hover{background:rgba(255,255,255,0.1);border-color:rgba(255,255,255,0.5)}
.debug-panel{position:fixed;bottom:50px;left:10px;background:rgba(0,0,0,0.9);color:#fff;padding:12px;border-radius:8px;font-size:11px;font-family:monospace;max-width:300px;max-height:200px;overflow-y:auto;display:none;z-index:1004;border:1px solid rgba(255,255,255,0.2)}
@media (max-width: 768px) {
    .status{font-size:12px;padding:6px 12px}
    .controls{bottom:60px;flex-direction:column;gap:4px}
    .btn{font-size:11px;padding:4px 8px}
}
</style>
</head><body>
<div id="loading">
  <div class="spinner"></div>
  <div>Loading Twitch stream...</div>
  <div style="font-size:12px;margin-top:10px;opacity:0.7">GitHub Pages HTTPS</div>
</div>
<div id="status" class="status">🌐 Initializing...</div>
<div id="latency" class="latency-display">Latency: --</div>
<div id="twitch"></div>

<div class="controls">
  <button class="btn" onclick="toggleLatencyDisplay()">📊</button>
  <button class="btn" onclick="toggleDebugPanel()">🔧</button>
  <button class="btn" onclick="refreshStream()">🔄</button>
</div>

<div id="debug" class="debug-panel">
  <div id="debug-content">Debug info will appear here...</div>
</div>

<script>
console.log('🌐 Twitch GitHub Pages Embed v2.0');
console.log('📍 URL:', window.location.href);
console.log('🖥️ User Agent:', navigator.userAgent);

// Parse URL parameters
const urlParams = new URLSearchParams(window.location.search);
const channel = urlParams.get('channel') || 'Perrikaryal';
const quality = urlParams.get('quality') || '1080p';
const volume = parseFloat(urlParams.get('volume')) || 0.1;
const autoplay = urlParams.get('autoplay') !== 'false';
const muted = urlParams.get('muted') === 'true';
const debug = urlParams.get('debug') === 'true';

// Configuration
const config = {
    channel: channel,
    quality: quality,
    volume: volume,
    autoplay: autoplay,
    muted: muted,
    debug: debug,
    latencyInterval: 1000,
    latencyTimeout: 5000
};

console.log('⚙️ Config:', config);

// Global variables
let statusDiv = document.getElementById('status');
let loadingDiv = document.getElementById('loading');
let latencyDiv = document.getElementById('latency');
let debugDiv = document.getElementById('debug');
let debugContent = document.getElementById('debug-content');

let player = null;
let latencyMonitor = null;
let currentLatency = -1;
let lastLatencyUpdate = 0;
let playerReady = false;
let latencyDisplayVisible = false;
let debugPanelVisible = false;

// Debug logging
const debugLogs = [];
function debugLog(message) {
    const timestamp = new Date().toLocaleTimeString();
    const logEntry = `[${timestamp}] ${message}`;
    debugLogs.push(logEntry);
    console.log(logEntry);
    
    if (debugPanelVisible && debugContent) {
        debugContent.innerHTML = debugLogs.slice(-20).join('<br>');
        debugContent.scrollTop = debugContent.scrollHeight;
    }
}

// Status management
function updateStatus(msg, type = '') {
    debugLog(`Status: ${msg} (${type})`);
    if (statusDiv) {
        statusDiv.innerHTML = '🌐 ' + msg;
        statusDiv.className = 'status' + (type ? ' ' + type : '');
    }
}

// Latency display
function updateLatencyDisplay(latency) {
    if (latencyDiv) {
        if (latency >= 0) {
            latencyDiv.innerHTML = `Latency: ${latency.toFixed(2)}s`;
            latencyDiv.style.color = latency < 3 ? '#00ff00' : latency < 6 ? '#ffff00' : '#ff6666';
        } else {
            latencyDiv.innerHTML = 'Latency: --';
            latencyDiv.style.color = '#666';
        }
    }
}

// Control functions
function toggleLatencyDisplay() {
    latencyDisplayVisible = !latencyDisplayVisible;
    if (latencyDiv) {
        latencyDiv.style.display = latencyDisplayVisible ? 'block' : 'none';
    }
    debugLog(`Latency display: ${latencyDisplayVisible ? 'shown' : 'hidden'}`);
}

function toggleDebugPanel() {
    debugPanelVisible = !debugPanelVisible;
    if (debugDiv) {
        debugDiv.style.display = debugPanelVisible ? 'block' : 'none';
        if (debugPanelVisible && debugContent) {
            debugContent.innerHTML = debugLogs.slice(-20).join('<br>');
            debugContent.scrollTop = debugContent.scrollHeight;
        }
    }
    debugLog(`Debug panel: ${debugPanelVisible ? 'shown' : 'hidden'}`);
}

function refreshStream() {
    debugLog('Refreshing stream...');
    if (player) {
        try {
            player.pause();
            setTimeout(() => {
                if (player) player.play();
            }, 500);
            updateStatus('Stream refreshed', 'info');
        } catch (e) {
            debugLog(`Refresh error: ${e.message}`);
            updateStatus('Refresh failed', 'error');
        }
    }
}

// Unity communication
window.getLatency = function() {
    return currentLatency;
};

window.getPlayerState = function() {
    if (!player || !playerReady) return 'not_ready';
    try {
        if (player.isPaused()) return 'paused';
        if (player.getEnded()) return 'ended';
        return 'playing';
    } catch (e) {
        return 'error';
    }
};

window.setVolume = function(vol) {
    if (player && playerReady) {
        try {
            player.setVolume(Math.max(0, Math.min(1, vol)));
            debugLog(`Volume set to: ${vol}`);
            return true;
        } catch (e) {
            debugLog(`Volume error: ${e.message}`);
            return false;
        }
    }
    return false;
};

window.setQuality = function(qual) {
    if (player && playerReady) {
        try {
            const qualities = player.getQualities();
            const target = qualities.find(q => 
                q.name && q.name.toLowerCase().includes(qual.toLowerCase())
            ) || qualities.find(q => q.name === 'auto') || qualities[0];
            
            if (target) {
                player.setQuality(target.group || target.name);
                debugLog(`Quality set to: ${target.name}`);
                return true;
            }
        } catch (e) {
            debugLog(`Quality error: ${e.message}`);
        }
    }
    return false;
};

window.playPause = function() {
    if (player && playerReady) {
        try {
            if (player.isPaused()) {
                player.play();
                debugLog('Player resumed');
                return 'playing';
            } else {
                player.pause();
                debugLog('Player paused');
                return 'paused';
            }
        } catch (e) {
            debugLog(`Play/pause error: ${e.message}`);
            return 'error';
        }
    }
    return 'not_ready';
};

// Unity message handling
window.addEventListener('unitydata', function(e) {
    debugLog(`Unity message: ${e.detail}`);
    
    if (e.detail === 'getLatency' && typeof window.sendToUnity === 'function') {
        try {
            window.sendToUnity('lat:' + currentLatency);
        } catch (err) {
            debugLog(`Unity latency response error: ${err.message}`);
            window.sendToUnity('lat:-1');
        }
    } else if (e.detail === 'getState' && typeof window.sendToUnity === 'function') {
        try {
            window.sendToUnity('state:' + window.getPlayerState());
        } catch (err) {
            window.sendToUnity('state:error');
        }
    } else if (e.detail.startsWith('setVolume:') && typeof window.sendToUnity === 'function') {
        const vol = parseFloat(e.detail.substring(10));
        const success = window.setVolume(vol);
        window.sendToUnity('volume:' + (success ? 'ok' : 'error'));
    } else if (e.detail.startsWith('setQuality:') && typeof window.sendToUnity === 'function') {
        const qual = e.detail.substring(11);
        const success = window.setQuality(qual);
        window.sendToUnity('quality:' + (success ? 'ok' : 'error'));
    } else if (e.detail === 'playPause' && typeof window.sendToUnity === 'function') {
        const state = window.playPause();
        window.sendToUnity('playPause:' + state);
    }
});

// Latency monitoring
function startLatencyMonitoring() {
    if (latencyMonitor) clearInterval(latencyMonitor);
    
    latencyMonitor = setInterval(() => {
        if (!player || !playerReady) {
            currentLatency = -1;
            updateLatencyDisplay(currentLatency);
            return;
        }
        
        try {
            const stats = player.getPlaybackStats();
            if (stats && typeof stats.hlsLatencyBroadcaster !== 'undefined') {
                currentLatency = stats.hlsLatencyBroadcaster;
                lastLatencyUpdate = Date.now();
                updateLatencyDisplay(currentLatency);
                
                // Send to Unity if available
                if (typeof window.sendToUnity === 'function') {
                    try {
                        window.sendToUnity('latency:' + currentLatency);
                    } catch (e) {
                        // Silent fail - Unity might not be listening
                    }
                }
            } else {
                // Check if data is stale
                if (Date.now() - lastLatencyUpdate > config.latencyTimeout) {
                    currentLatency = -1;
                    updateLatencyDisplay(currentLatency);
                }
            }
        } catch (e) {
            debugLog(`Latency monitoring error: ${e.message}`);
            currentLatency = -1;
            updateLatencyDisplay(currentLatency);
        }
    }, config.latencyInterval);
    
    debugLog('Latency monitoring started');
}

function stopLatencyMonitoring() {
    if (latencyMonitor) {
        clearInterval(latencyMonitor);
        latencyMonitor = null;
        currentLatency = -1;
        updateLatencyDisplay(currentLatency);
        debugLog('Latency monitoring stopped');
    }
}

// Initialize Twitch player
function initializeTwitchPlayer() {
    debugLog('Initializing Twitch player...');
    updateStatus('Creating Twitch player...', '');
    
    try {
        player = new Twitch.Player('twitch', {
            channel: config.channel,
            quality: 'auto',
            width: '100%',
            height: '100%',
            autoplay: config.autoplay,
            muted: config.muted,
            volume: config.volume,
            parent: [window.location.hostname],
            allowfullscreen: false
        });
        
        updateStatus('Player created, waiting for ready...', 'info');
        debugLog(`Player config: channel=${config.channel}, quality=auto, volume=${config.volume}, autoplay=${config.autoplay}, muted=${config.muted}`);
        
        // Event listeners
        player.addEventListener(Twitch.Player.READY, function() {
            debugLog('🎉 Twitch player ready!');
            playerReady = true;
            updateStatus('Player ready!', 'success');
            if (loadingDiv) loadingDiv.style.display = 'none';
            
            // Start latency monitoring
            startLatencyMonitoring();
            
            // Set initial quality after a delay
            setTimeout(() => {
                try {
                    const qualities = player.getQualities();
                    debugLog(`Available qualities: ${qualities.map(q => q.name).join(', ')}`);
                    
                    const targetQuality = config.quality;
                    const matchedQuality = qualities.find(q => 
                        q.name && q.name.toLowerCase().includes(targetQuality.toLowerCase())
                    ) || qualities.find(q => q.name === 'auto') || qualities[0];
                    
                    if (matchedQuality) {
                        player.setQuality(matchedQuality.group || matchedQuality.name);
                        debugLog(`Quality set to: ${matchedQuality.name}`);
                        updateStatus(`🎮 ${matchedQuality.name}`, 'success');
                        
                        // Show latency display if debug mode
                        if (config.debug) {
                            toggleLatencyDisplay();
                            toggleDebugPanel();
                        }
                    }
                } catch (e) {
                    debugLog(`Quality setting error: ${e.message}`);
                    updateStatus('Quality setting failed', 'warning');
                }
            }, 2000);
        });

        player.addEventListener(Twitch.Player.PLAYING, function() {
            debugLog('▶️ Stream playing');
            updateStatus('🎮 Playing', 'success');
        });

        player.addEventListener(Twitch.Player.PAUSE, function() {
            debugLog('⏸️ Stream paused');
            updateStatus('⏸️ Paused', 'warning');
        });

        player.addEventListener(Twitch.Player.ENDED, function() {
            debugLog('🏁 Stream ended');
            updateStatus('Stream ended', 'info');
            stopLatencyMonitoring();
        });

        player.addEventListener(Twitch.Player.OFFLINE, function() {
            debugLog('📺 Stream offline');
            updateStatus('📺 Stream offline', 'warning');
            stopLatencyMonitoring();
        });

        // Send ready signal to Unity
        if (typeof window.sendToUnity === 'function') {
            try {
                window.sendToUnity('ready');
            } catch (e) {
                debugLog(`Unity ready signal error: ${e.message}`);
            }
        }

    } catch (e) {
        debugLog(`❌ Player initialization error: ${e.message}`);
        updateStatus(`Player creation failed: ${e.message}`, 'error');
        if (loadingDiv) {
            loadingDiv.innerHTML = `<div style="color:#ff6b6b">❌ Failed to create player<br><small>${e.message}</small></div>`;
        }
    }
}

// Load Twitch SDK
debugLog('Loading Twitch SDK...');
updateStatus('Loading Twitch SDK...', '');

const script = document.createElement('script');
script.src = 'https://player.twitch.tv/js/embed/v1.js';
script.onload = function() {
    debugLog('✅ Twitch SDK loaded successfully');
    updateStatus('SDK loaded, creating player...', 'info');
    initializeTwitchPlayer();
};
script.onerror = function() {
    debugLog('❌ Failed to load Twitch SDK');
    updateStatus('Failed to load Twitch SDK', 'error');
    if (loadingDiv) {
        loadingDiv.innerHTML = '<div style="color:#ff6b6b">❌ Failed to load Twitch SDK<br><small>Check internet connection</small></div>';
    }
};
document.head.appendChild(script);

// Global error handling
window.addEventListener('error', function(e) {
    const errorMsg = e.error ? e.error.message : e.message;
    debugLog(`🚨 Global error: ${errorMsg} (${e.filename}:${e.lineno})`);
    updateStatus(`Error: ${errorMsg}`, 'error');
});

window.addEventListener('unhandledrejection', function(e) {
    debugLog(`🚨 Unhandled promise rejection: ${e.reason}`);
    updateStatus(`Promise error: ${e.reason}`, 'error');
});

// Hide status after successful loading
setTimeout(() => {
    if (statusDiv && statusDiv.textContent.includes('🎮') && !config.debug) {
        statusDiv.style.transition = 'opacity 1s';
        statusDiv.style.opacity = '0.3';
        setTimeout(() => {
            if (statusDiv && statusDiv.textContent.includes('🎮') && !config.debug) {
                statusDiv.style.display = 'none';
            }
        }, 3000);
    }
}, 8000);

// Cleanup on page unload
window.addEventListener('beforeunload', function() {
    stopLatencyMonitoring();
    if (player) {
        try {
            player.pause();
        } catch (e) {
            // Silent cleanup
        }
    }
});

debugLog('🌐 GitHub Pages Twitch embed initialized');
</script></body></html>
