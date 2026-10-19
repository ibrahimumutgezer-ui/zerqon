```react
import React, { useState, useEffect, useRef } from 'react';
import { 
  Hash, Volume2, Plus, Settings, Mic, MicOff, Headphones, 
  MessageSquare, Users, Search, Send, Moon, Sun, 
  X, Lock, PhoneCall, Check, Shield, Gamepad2, Trophy, HelpCircle,
  RotateCcw, Monitor, MonitorOff, Play, Dices, Award, Zap,
  Flame, Crosshair, Sword, Radio, Crown, Video, VideoOff,
  UserPlus, Share2, CheckCircle2, PhoneOff, Image, Smile, Paperclip, MoreVertical,
  VolumeX, Volume1, Sparkles
} from 'lucide-react';

// --- ALL REQUESTED GAMES METADATA ---
const REQUESTED_GAMES = [
  // Tactical / Shooter
  { id: 'valo', name: 'VALORANT', genre: 'Tactical Shooter', category: 'fps', icon: '🔫', bg: 'from-rose-600 to-red-700', desc: 'Dereceli, Taktiksel ajanlar ve Spike lobisi' },
  { id: 'cs2', name: 'Counter-Strike (CS2)', genre: 'FPS', category: 'fps', icon: '🎯', bg: 'from-amber-600 to-zinc-800', desc: 'Seçkin Rekabetçi, Yoldaş ve FACEIT' },
  { id: 'fortnite', name: 'Fortnite', genre: 'Battle Royale', category: 'fps', icon: '🪂', bg: 'from-purple-500 to-indigo-600', desc: 'Sıfır İnşa ve Zafer Tacı lobileri' },
  { id: 'codm', name: 'Call of Duty: Mobile', genre: 'FPS / BR', category: 'fps', icon: '🪖', bg: 'from-stone-700 to-zinc-900', desc: 'Multiplayer haritalar ve BR squads' },
  { id: 'freefire', name: 'Free Fire', genre: 'Battle Royale', category: 'fps', icon: '🔥', bg: 'from-orange-600 to-red-600', desc: 'Hızlı 10 dakikalık hayatta kalma' },
  { id: 'pubg', name: 'PUBG: Battlegrounds', genre: 'Battle Royale', category: 'fps', icon: '🪖', bg: 'from-yellow-600 to-stone-800', desc: 'Çorba Parası Squad Maçlar' },
  { id: 'apex', name: 'Apex Legends', genre: 'Hero Shooter', category: 'fps', icon: '⚡', bg: 'from-red-600 to-orange-600', desc: 'Hızlı BR ve Arena takımları' },

  // Racing
  { id: 'nfs', name: 'Need for Speed Heat', genre: 'Racing', category: 'racing', icon: '🏎️', bg: 'from-purple-600 to-pink-600', desc: 'Sokak yarışları ve polis takipleri' },
  { id: 'asphalt', name: 'Asphalt Legends Unite', genre: 'Racing', category: 'racing', icon: '🏎️', bg: 'from-blue-600 to-cyan-500', desc: 'Arcade hiper araba yarışları' },
  { id: 'carx', name: 'CarX Street', genre: 'Racing', category: 'racing', icon: '🚗', bg: 'from-amber-600 to-red-600', desc: 'Gece drifteri ve modifiye dünyası' },
  { id: 'forza', name: 'Forza Horizon', genre: 'Racing', category: 'racing', icon: '🏁', bg: 'from-orange-500 to-amber-500', desc: 'Açık dünya festival ve konvoyları' },
  
  // RPG / Open World
  { id: 'elden', name: 'Elden Ring', genre: 'RPG / Souls', category: 'rpg', icon: '⚔️', bg: 'from-amber-700 to-yellow-600', desc: 'Boss savaşları ve Elden Lordu macerası' },
  { id: 'rdr2', name: 'Red Dead Redemption 2', genre: 'Open World', category: 'rpg', icon: '🤠', bg: 'from-red-800 to-amber-900', desc: 'Vahşi batı çetesi ve Roleplay sunucusu' },
  { id: 'gtav', name: 'GTA V', genre: 'Open World', category: 'rpg', icon: '💣', bg: 'from-emerald-700 to-teal-800', desc: 'FiveM Roleplay ve soygun ekipleri' },
  { id: 'genshin', name: 'Genshin Impact', genre: 'Anime RPG', category: 'rpg', icon: '✨', bg: 'from-sky-500 to-indigo-600', desc: 'Element savaşları ve Co-Op zindanları' },
  { id: 'mc', name: 'Minecraft', genre: 'Sandbox', category: 'rpg', icon: '🟩', bg: 'from-green-600 to-emerald-700', desc: 'Survival, Skyblock ve üretken build' },
  { id: 'roblox', name: 'Roblox', genre: 'Sandbox / Games', category: 'rpg', icon: '🟥', bg: 'from-red-500 to-rose-700', desc: 'Çeşitli oyuncu topluluk haritaları' },

  // Sports / MOBA / Strategy
  { id: 'fc26', name: 'EA Sports FC 26', genre: 'Sports', category: 'sports_moba', icon: '⚽', bg: 'from-blue-700 to-indigo-900', desc: 'Pro Clubs, Ultimate Team ve 1v1 maçlar' },
  { id: 'lol', name: 'League of Legends', genre: '5v5 MOBA', category: 'sports_moba', icon: '🛡️', bg: 'from-cyan-600 to-blue-800', desc: 'Sihirdar Vadisi ve ARAM kapışmaları' },
  { id: 'mlbb', name: 'Mobile Legends Bang Bang', genre: '5v5 MOBA', category: 'sports_moba', icon: '⚔️', bg: 'from-blue-600 to-purple-700', desc: '5v5 Koridor savaşları ve Mythic ligi' },
  { id: 'brawl', name: 'Brawl Stars', genre: '3v3 MOBA', category: 'sports_moba', icon: '⭐', bg: 'from-yellow-500 to-amber-600', desc: '3v3 Kupa kasma ve Kulüp Ligi' },
  { id: 'clash', name: 'Clash Royale', genre: '1v1 Strategy', category: 'sports_moba', icon: '👑', bg: 'from-indigo-600 to-purple-800', desc: 'Kule yıkma ve kart düelloları' }
];

// Initial Friends List for DMs & Voice Lobbies
const INITIAL_FRIENDS = [
  { id: 'dm-ahmet', name: 'Ahmet', tag: '1024', status: 'online', avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?auto=format&fit=crop&w=120&q=80', customStatus: 'VALORANT Ranked 🏆', playingGame: 'VALORANT', volume: 100, isMuted: false, isSpeaking: true },
  { id: 'dm-mehmet', name: 'Mehmet', tag: '3049', status: 'online', avatar: 'https://images.unsplash.com/photo-1570295999919-56ceb5ecca61?auto=format&fit=crop&w=120&q=80', customStatus: 'EA FC 26 Kadro Kuruyor ⚽', playingGame: 'EA Sports FC 26', volume: 120, isMuted: false, isSpeaking: false },
  { id: 'dm-zeynep', name: 'Zeynep', tag: '8812', status: 'idle', avatar: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&w=120&q=80', customStatus: 'NFS Heat Drift Basıyor 🏎️', playingGame: 'Need for Speed Heat', volume: 90, isMuted: false, isSpeaking: true },
  { id: 'dm-ayse', name: 'Ayşe', tag: '4011', status: 'dnd', avatar: 'https://images.unsplash.com/photo-1438761681033-6461ffad8d80?auto=format&fit=crop&w=120&q=80', customStatus: 'Genshin Impact Zindan 💫', playingGame: 'Genshin Impact', volume: 100, isMuted: true, isSpeaking: false },
  { id: 'dm-can', name: 'Can', tag: '9002', status: 'online', avatar: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?auto=format&fit=crop&w=120&q=80', customStatus: 'Elden Ring Boss Savaşı ⚔️', playingGame: 'Elden Ring', volume: 150, isMuted: false, isSpeaking: false },
  { id: 'dm-elif', name: 'Elif', tag: '5543', status: 'offline', avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=120&q=80', customStatus: 'Minecraft Survival 🟩', playingGame: 'Minecraft', volume: 80, isMuted: false, isSpeaking: false }
];

// Initial Custom Private Groups
const INITIAL_GROUPS = [
  { id: 'grp-1', name: '🔒 VALORANT Ranked Ekibi', isGroup: true, members: ['Ahmet', 'Zeynep', 'DevUser'], avatar: '🎯' },
  { id: 'grp-2', name: '⚽ EA FC 26 Turnuva Grubu', isGroup: true, members: ['Mehmet', 'Can', 'DevUser'], avatar: '⚽' }
];

// Initial Zerqon Server Setup
const INITIAL_SERVERS = [
  {
    id: 's-gaming',
    name: 'Zerqon Gaming Hub',
    icon: '⚡',
    bg: 'bg-gradient-to-r from-purple-600 to-indigo-600',
    categories: [
      {
        name: '🎮 OYUN ODALARI',
        channels: REQUESTED_GAMES.map(game => ({
          id: `ch-game-${game.id}`,
          name: game.name.toLowerCase().replace(/[^a-z0-9]/g, '-'),
          displayName: game.name,
          type: 'game_simulator',
          gameData: game,
          topic: `${game.name} - Sesli Lobi, Mini Oyun & Canlı Chat`
        }))
      },
      {
        name: '💬 GENEL SOHBET',
        channels: [
          { id: 'ch-genel', name: 'genel-sohbet', displayName: 'Genel Sohbet', type: 'text', topic: 'Zerqon topluluk sohbet ve muhabbet alanı' },
          { id: 'ch-duyurular', name: 'duyurular', displayName: 'Duyurular', type: 'text', topic: 'Turnuva ve etkinlik haberleri' }
        ]
      },
      {
        name: '🔊 SES ODALARI',
        channels: [
          { id: 'v-valo-lobby', name: '🔊 VALORANT Sesli Lobi', displayName: 'VALORANT Sesli Lobi', type: 'voice', usersConnected: ['Ahmet', 'Zeynep'] },
          { id: 'v-squad1', name: '🔊 Squad Voice #1', displayName: 'Squad Voice #1', type: 'voice', usersConnected: ['Mehmet', 'Can'] }
        ]
      }
    ]
  }
];

// Messages mapping per DM/Group/Channel
const INITIAL_MESSAGES = {
  'dm-ahmet': [
    { id: 'm1', sender: { name: 'Ahmet', tag: '1024', avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?auto=format&fit=crop&w=120&q=80' }, timestamp: '14:10', content: 'Selam knk! Zerqon da VALORANT sesli odaya gel, ses düzeyini ayarlayalım!' },
    { id: 'm2', sender: { name: 'DevUser (Sen)', tag: '0007', avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?auto=format&fit=crop&w=120&q=80' }, timestamp: '14:12', content: 'Geliyorum knk, Zerqon da mikrofonu açtım!' }
  ],
  'grp-1': [
    { id: 'm3', sender: { name: 'Ahmet', tag: '1024', avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?auto=format&fit=crop&w=120&q=80' }, timestamp: '15:00', content: 'VALORANT tayfası hazır mı? Zerqon sesliye geçtik!' }
  ],
  'ch-game-valo': [
    { id: 'm4', sender: { name: 'Ahmet', tag: '1024', avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?auto=format&fit=crop&w=120&q=80' }, timestamp: '15:10', content: 'VALORANT odasındayız beyler! "Sesli Odaya Katıl" butonuna basıp gelin 🔥' }
  ]
};

// Web Audio sound generator
const playSound = (type) => {
  try {
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain);
    gain.connect(ctx.destination);

    if (type === 'click') {
      osc.type = 'sine';
      osc.frequency.setValueAtTime(600, ctx.currentTime);
      gain.gain.setValueAtTime(0.03, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.05);
      osc.start();
      osc.stop(ctx.currentTime + 0.05);
    } else if (type === 'voice_join') {
      osc.type = 'triangle';
      osc.frequency.setValueAtTime(440, ctx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(880, ctx.currentTime + 0.15);
      gain.gain.setValueAtTime(0.06, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.15);
      osc.start();
      osc.stop(ctx.currentTime + 0.15);
    } else if (type === 'shot') {
      osc.type = 'square';
      osc.frequency.setValueAtTime(800, ctx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(100, ctx.currentTime + 0.1);
      gain.gain.setValueAtTime(0.08, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.1);
      osc.start();
      osc.stop(ctx.currentTime + 0.1);
    }
  } catch (e) {}
};

export default function App() {
  // Navigation State
  const [activeServerId, setActiveServerId] = useState('s-gaming');
  const [activeDmOrGroupId, setActiveDmOrGroupId] = useState('dm-ahmet');
  const [activeChannelId, setActiveChannelId] = useState('ch-game-valo');

  // Application Data State
  const [friends, setFriends] = useState(INITIAL_FRIENDS);
  const [groups, setGroups] = useState(INITIAL_GROUPS);
  const [servers, setServers] = useState(INITIAL_SERVERS);
  const [messages, setMessages] = useState(INITIAL_MESSAGES);
  const [inputMessage, setInputMessage] = useState('');

  // Active Voice Connection State
  const [activeVoiceRoom, setActiveVoiceRoom] = useState({ id: 'v-valo-lobby', name: 'VALORANT Sesli Lobi', connected: true });
  
  // Controls & Modals
  const [showCreateGroup, setShowCreateGroup] = useState(false);
  const [showLobbyModal, setShowLobbyModal] = useState(false);
  const [showMembers, setShowMembers] = useState(true);
  const [theme, setTheme] = useState('dark');
  
  // User Mic/Audio State
  const [isMuted, setIsMuted] = useState(false);
  const [isDeafened, setIsDeafened] = useState(false);
  const [isCameraOn, setIsCameraOn] = useState(false);
  const [isScreenSharing, setIsScreenSharing] = useState(false);

  // Form States
  const [newGroupName, setNewGroupName] = useState('');
  const [selectedFriendsForGroup, setSelectedFriendsForGroup] = useState([]);

  // Mini-Game Simulators State
  const [fpsScore, setFpsScore] = useState(0);
  const [fpsTarget, setFpsTarget] = useState({ x: 50, y: 50 });
  const [racingSpeed, setRacingSpeed] = useState(0);
  const [racingScore, setRacingScore] = useState(0);

  const chatEndRef = useRef(null);

  const isHome = activeServerId === 'home';
  const activeServer = servers.find(s => s.id === activeServerId);

  // Resolve current active channel
  let activeChannel = null;
  if (activeServer) {
    for (const cat of activeServer.categories) {
      const found = cat.channels.find(c => c.id === activeChannelId);
      if (found) {
        activeChannel = found;
        break;
      }
    }
  }

  // Active DM / Group
  const activeFriend = friends.find(f => f.id === activeDmOrGroupId);
  const activeGroup = groups.find(g => g.id === activeDmOrGroupId);

  // Auto Scroll Chat
  useEffect(() => {
    chatEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages, activeChannelId, activeDmOrGroupId]);

  // Per-User Volume Slider Handler
  const handleUserVolumeChange = (friendId, newVolume) => {
    setFriends(prev => prev.map(f => f.id === friendId ? { ...f, volume: newVolume, isMuted: newVolume === 0 } : f));
  };

  const handleUserMuteToggle = (friendId) => {
    setFriends(prev => prev.map(f => f.id === friendId ? { ...f, isMuted: !f.isMuted } : f));
  };

  // Join Voice Room Handler
  const handleConnectVoice = (roomName) => {
    playSound('voice_join');
    setActiveVoiceRoom({ id: 'v-' + Date.now(), name: roomName, connected: true });
  };

  const handleDisconnectVoice = () => {
    playSound('click');
    setActiveVoiceRoom({ id: null, name: '', connected: false });
    setIsScreenSharing(false);
  };

  // Send Message
  const handleSendMessage = (e) => {
    e?.preventDefault();
    if (!inputMessage.trim()) return;

    const targetKey = isHome ? activeDmOrGroupId : activeChannel?.id;
    if (!targetKey) return;

    const timeNow = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    const newMsg = {
      id: 'm-' + Date.now(),
      sender: {
        name: 'DevUser (Sen)',
        tag: '0007',
        avatar: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?auto=format&fit=crop&w=120&q=80'
      },
      timestamp: timeNow,
      content: inputMessage
    };

    setMessages(prev => ({
      ...prev,
      [targetKey]: [...(prev[targetKey] || []), newMsg]
    }));

    playSound('click');
    const textSent = inputMessage;
    setInputMessage('');

    if (textSent.startsWith('/')) {
      setTimeout(() => {
        const botMsg = {
          id: 'bot-' + Date.now(),
          sender: { name: 'ZerqonBot 🤖', tag: '0001', avatar: 'https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?auto=format&fit=crop&w=120&q=80' },
          timestamp: timeNow,
          content: `🤖 **Zerqon Komutu İşlendi:** ${textSent} - [VALORANT Sesli Odası Aktif]`
        };
        setMessages(p => ({ ...p, [targetKey]: [...(p[targetKey] || []), botMsg] }));
      }, 400);
    }
  };

  // Create Custom Private Group
  const handleCreateGroup = (e) => {
    e.preventDefault();
    if (!newGroupName.trim()) return;

    const newGrp = {
      id: 'grp-' + Date.now(),
      name: `🔒 ${newGroupName}`,
      isGroup: true,
      members: [...selectedFriendsForGroup, 'DevUser'],
      avatar: '🛡️'
    };

    setGroups(g => [...g, newGrp]);
    setActiveServerId('home');
    setActiveDmOrGroupId(newGrp.id);
    setNewGroupName('');
    setSelectedFriendsForGroup([]);
    setShowCreateGroup(false);
    playSound('voice_join');
  };

  return (
    <div className={`flex h-screen w-screen overflow-hidden text-sm font-sans select-none ${theme === 'dark' ? 'bg-[#313338] text-gray-200' : 'bg-gray-100 text-gray-800'}`}>
      
      {/* 1. LEFTMOST SERVER BAR WITH PROMINENT ZERQON BRANDING AT THE TOP */}
      <div className={`w-[80px] flex flex-col items-center py-3 space-y-2 shrink-0 z-20 ${theme === 'dark' ? 'bg-[#1e1f22]' : 'bg-gray-300'}`}>
        
        {/* PROMINENT ZERQON LOGO AT TOP LEFT */}
        <div className="flex flex-col items-center justify-center w-full mb-2">
          <div className="w-12 h-12 rounded-2xl bg-gradient-to-tr from-purple-600 via-indigo-500 to-pink-500 flex items-center justify-center shadow-lg shadow-purple-500/30 transform hover:scale-105 transition-all duration-300 cursor-pointer">
            <Sparkles className="w-6 h-6 text-white animate-pulse" />
          </div>
          <span className="text-[11px] font-black tracking-widest text-transparent bg-clip-text bg-gradient-to-r from-purple-400 via-indigo-300 to-pink-400 uppercase mt-1">
            Zerqon
          </span>
        </div>

        <div className="w-10 h-[2px] bg-[#35363c] rounded my-1" />

        <div className="relative group flex items-center justify-center w-full">
          <div className={`absolute left-0 w-1 rounded-r-full transition-all duration-300 ${isHome ? 'h-10 bg-white' : 'h-0 group-hover:h-5 bg-white'}`} />
          <button 
            onClick={() => { setActiveServerId('home'); setActiveDmOrGroupId('dm-ahmet'); playSound('click'); }}
            className={`w-12 h-12 rounded-[24px] hover:rounded-[16px] flex items-center justify-center transition-all duration-300 ${isHome ? 'bg-[#5865f2] text-white' : 'bg-[#313338] hover:bg-[#5865f2] text-gray-200'}`}
            title="Zerqon DM & Özel Gruplar"
          >
            <MessageSquare className="w-6 h-6" />
          </button>
        </div>

        {servers.map((server) => {
          const isActive = activeServerId === server.id;
          return (
            <div key={server.id} className="relative group flex items-center justify-center w-full">
              <div className={`absolute left-0 w-1 rounded-r-full transition-all duration-300 ${isActive ? 'h-10 bg-white' : 'h-0 group-hover:h-5 bg-white'}`} />
              <button
                onClick={() => {
                  setActiveServerId(server.id);
                  setActiveChannelId(server.categories[0].channels[0].id);
                  playSound('click');
                }}
                className={`w-12 h-12 rounded-[24px] hover:rounded-[16px] flex items-center justify-center text-lg font-bold transition-all duration-300 text-white shadow-md ${server.bg} ${isActive ? 'rounded-[16px] ring-2 ring-indigo-400' : 'hover:opacity-90'}`}
              >
                {server.icon}
              </button>
            </div>
          );
        })}

        <div className="flex-1" />

        <button
          onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}
          className="w-10 h-10 rounded-full bg-[#313338] hover:bg-amber-500 text-amber-400 hover:text-white flex items-center justify-center transition"
          title="Tema Değiştir"
        >
          {theme === 'dark' ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5 text-gray-700" />}
        </button>
      </div>

      {/* 2. CHANNELS & DMs SIDEBAR */}
      <div className={`w-64 flex flex-col shrink-0 ${theme === 'dark' ? 'bg-[#2b2d31]' : 'bg-gray-200 border-r border-gray-300'}`}>
        
        {/* HEADER WITH ZERQON BRANDING */}
        <div className={`h-12 px-4 border-b flex items-center justify-between font-bold text-sm shadow-sm ${theme === 'dark' ? 'border-[#1f2023]' : 'border-gray-300'}`}>
          <div className="flex items-center space-x-2">
            <span className="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-indigo-400 font-extrabold text-base">Zerqon</span>
            <span className="text-gray-500 text-xs">| {isHome ? 'Mesajlar' : activeServer?.name}</span>
          </div>
          {isHome && (
            <button 
              onClick={() => setShowCreateGroup(true)}
              className="p-1 hover:bg-white/10 text-amber-400 rounded flex items-center space-x-1"
              title="Grup Aç"
            >
              <Plus className="w-4 h-4" />
            </button>
          )}
        </div>

        <div className="flex-1 overflow-y-auto px-2 py-3 space-y-4 no-scrollbar">
          {isHome ? (
       
