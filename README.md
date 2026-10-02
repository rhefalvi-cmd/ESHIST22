<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EsHist - Eksplorasi Sejarah Interaktif</title>
    <!-- Tailwind CSS CDN for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        nationalRed: '#DC2626',
                        nationalDarkRed: '#991B1B',
                        nationalCream: '#FDFBF7',
                        nationalCreamCard: '#F4F0E6',
                        nationalDark: '#1F2937',
                    },
                    fontFamily: {
                        sans: ['system-ui', '-apple-system', 'BlinkMacSystemFont', '"Segoe UI"', 'Roboto', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #FDFBF7;
            color: #1F2937;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            overflow-x: hidden;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #F4F0E6;
        }
        ::-webkit-scrollbar-thumb {
            background: #DC2626;
            border-radius: 3px;
        }
        .page-section {
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
        }
        .page-section.active {
            display: block;
            opacity: 1;
        }
        .sidebar-item {
            transition: all 0.2s ease;
        }
        .sidebar-item.active {
            background-color: rgba(220, 38, 38, 0.1);
            color: #DC2626;
            border-left: 4px solid #DC2626;
            font-weight: 700;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(6px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fadeIn 0.35s ease forwards;
        }
        .card-shadow {
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.05);
        }
    </style>
</head>
<body class="bg-nationalCream text-nationalDark min-h-screen flex flex-col md:flex-row selection:bg-nationalRed selection:text-white">

    <!-- Sidebar for Desktop & Tablet -->
    <aside id="sidebar" class="hidden md:flex flex-col w-64 bg-white border-r border-gray-200 h-screen sticky top-0 z-30 shadow-sm justify-between">
        <div class="p-6">
            <!-- App Logo -->
            <div class="flex items-center space-x-3 mb-8">
                <div class="w-10 h-10 bg-nationalRed rounded-2xl flex items-center justify-center text-white font-black text-xl shadow-md">
                    📚
                </div>
                <div>
                    <h1 class="text-2xl font-black tracking-tight text-nationalDark">Es<span class="text-nationalRed">Hist</span></h1>
                    <span class="text-[10px] uppercase tracking-wider text-gray-500 font-bold">Eksplorasi Sejarah</span>
                </div>
            </div>

            <!-- Menu Navigation -->
            <nav class="space-y-1">
                <div class="text-[10px] font-extrabold text-gray-400 uppercase tracking-widest px-3 mb-2">Menu Utama</div>
                
                <button onclick="showPage('home')" id="nav-home" class="sidebar-item active w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold text-gray-600 hover:bg-red-50 hover:text-nationalRed text-left">
                    <span class="text-lg">🏠</span>
                    <span>Beranda</span>
                </button>
                
                <button onclick="showPage('timeline')" id="nav-timeline" class="sidebar-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold text-gray-600 hover:bg-red-50 hover:text-nationalRed text-left">
                    <span class="text-lg">⏳</span>
                    <span>Jejak Sejarah</span>
                </button>

                <button onclick="showPage('videos')" id="nav-videos" class="sidebar-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold text-gray-600 hover:bg-red-50 hover:text-nationalRed text-left">
                    <span class="text-lg">🎬</span>
                    <span>Video Edukasi</span>
                </button>
                
                <button onclick="showPage('materials')" id="nav-materials" class="sidebar-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold text-gray-600 hover:bg-red-50 hover:text-nationalRed text-left">
                    <span class="text-lg">📚</span>
                    <span>Materi Organisasi</span>
                </button>
                
                <button onclick="showPage('game')" id="nav-game" class="sidebar-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold text-gray-600 hover:bg-red-50 hover:text-nationalRed text-left">
                    <span class="text-lg">🎮</span>
                    <span>Memory Match</span>
                </button>
                
                <button onclick="showPage('evaluation')" id="nav-evaluation" class="sidebar-item w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold text-gray-600 hover:bg-red-50 hover:text-nationalRed text-left">
                    <span class="text-lg">📝</span>
                    <span>Evaluasi Siswa</span>
                </button>
            </nav>
        </div>

        <!-- Sidebar Footer / About -->
        <div class="p-6 border-t border-gray-100">
            <button onclick="showPage('about')" id="nav-about" class="sidebar-item w-full flex items-center space-x-3 px-4 py-2.5 rounded-xl text-sm font-semibold text-gray-600 hover:bg-gray-100 text-left">
                <span class="text-base">ℹ️</span>
                <span>Tentang EsHist</span>
            </button>
        </div>
    </aside>

    <!-- Mobile Bottom Navigation Bar -->
    <nav class="md:hidden fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 z-40 flex justify-around p-2 shadow-lg">
        <button onclick="showPage('home')" id="mob-home" class="flex flex-col items-center justify-center p-1.5 text-nationalRed">
            <span class="text-lg">🏠</span>
            <span class="text-[9px] font-bold mt-0.5">Beranda</span>
        </button>
        <button onclick="showPage('timeline')" id="mob-timeline" class="flex flex-col items-center justify-center p-1.5 text-gray-500">
            <span class="text-lg">⏳</span>
            <span class="text-[9px] font-bold mt-0.5">Jejak</span>
        </button>
        <button onclick="showPage('videos')" id="mob-videos" class="flex flex-col items-center justify-center p-1.5 text-gray-500">
            <span class="text-lg">🎬</span>
            <span class="text-[9px] font-bold mt-0.5">Video</span>
        </button>
        <button onclick="showPage('materials')" id="mob-materials" class="flex flex-col items-center justify-center p-1.5 text-gray-500">
            <span class="text-lg">📚</span>
            <span class="text-[9px] font-bold mt-0.5">Materi</span>
        </button>
        <button onclick="showPage('game')" id="mob-game" class="flex flex-col items-center justify-center p-1.5 text-gray-500">
            <span class="text-lg">🎮</span>
            <span class="text-[9px] font-bold mt-0.5">Game</span>
        </button>
        <button onclick="showPage('evaluation')" id="mob-evaluation" class="flex flex-col items-center justify-center p-1.5 text-gray-500">
            <span class="text-lg">📝</span>
            <span class="text-[9px] font-bold mt-0.5">Evaluasi</span>
        </button>
    </nav>

    <!-- Main Content Area -->
    <main class="flex-1 flex flex-col min-h-screen pb-20 md:pb-0">
        
        <!-- Topbar -->
        <header class="bg-white/80 backdrop-blur-md sticky top-0 z-20 border-b border-gray-200 px-6 py-4 flex items-center justify-between shadow-sm">
            <div class="flex items-center space-x-3">
                <span id="topbar-title" class="text-lg font-black text-nationalDark">Beranda</span>
                <span class="text-xs bg-red-100 text-nationalRed font-bold px-2.5 py-1 rounded-full uppercase tracking-wider hidden sm:inline-block">SMA Sejarah</span>
            </div>
            
            <div class="flex items-center space-x-4">
                <!-- Progress Widget -->
                <div class="bg-nationalCreamCard border border-gray-200 px-3 py-1.5 rounded-full flex items-center space-x-2 shadow-inner">
                    <span class="text-xs font-bold text-gray-600 hidden sm:inline">Progress Belajar:</span>
                    <span id="topbar-progress-text" class="text-xs font-black text-nationalRed">0%</span>
                    <div class="w-16 bg-gray-200 rounded-full h-2 overflow-hidden hidden sm:block">
                        <div id="topbar-progress-bar" class="bg-nationalRed h-full transition-all duration-300" style="width: 0%"></div>
                    </div>
                </div>

                <!-- Avatar User Icon -->
                <div class="w-9 h-9 bg-nationalRed text-white rounded-full flex items-center justify-center font-bold text-sm shadow-md">
                    🎓
                </div>
            </div>
        </header>

        <!-- Content Sections Container -->
        <div class="p-4 sm:p-8 max-w-6xl mx-auto w-full flex-1">

            <!-- 1. BERANDA PAGE -->
            <section id="page-home" class="page-section active animate-fade-in space-y-8">
                <!-- Hero Card -->
                <div class="relative overflow-hidden bg-gradient-to-r from-nationalDark to-gray-900 text-white rounded-3xl p-8 sm:p-12 shadow-2xl">
                    <div class="absolute -right-10 -bottom-10 opacity-10 text-9xl">🇮🇩</div>
                    <div class="relative z-10 max-w-2xl space-y-4">
                        <div class="inline-flex items-center space-x-2 bg-red-600/30 text-red-300 border border-red-500/40 px-3.5 py-1 rounded-full text-xs font-bold uppercase tracking-wider">
                            <span>✨ Platform Pembelajaran Interaktif SMA</span>
                        </div>
                        <h2 class="text-3xl sm:text-5xl font-black tracking-tight leading-tight">
                            Eksplorasi Pergerakan Nasional Indonesia
                        </h2>
                        <p class="text-gray-300 text-base sm:text-lg leading-relaxed">
                            “Kenali tokoh, organisasi, gagasan, video edukatif, dan peristiwa yang membentuk perjalanan menuju Indonesia merdeka.”
                        </p>
                        <div class="pt-2 flex flex-wrap gap-3">
                            <button onclick="showPage('timeline')" class="bg-nationalRed hover:bg-nationalDarkRed text-white font-bold px-6 py-3.5 rounded-2xl shadow-lg hover:shadow-xl transition transform hover:-translate-y-0.5 flex items-center space-x-2 text-sm">
                                <span>Mulai Eksplorasi</span>
                                <span class="text-base">→</span>
                            </button>
                            <button onclick="showPage('videos')" class="bg-white/10 hover:bg-white/20 text-white font-bold px-6 py-3.5 rounded-2xl border border-white/20 transition flex items-center space-x-2 text-sm">
                                <span>🎬 Tonton Video Edukasi</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Statistics Cards -->
                <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 card-shadow flex items-center space-x-4">
                        <div class="w-12 h-12 bg-red-100 text-nationalRed rounded-2xl flex items-center justify-center text-xl font-bold">🏛️</div>
                        <div>
                            <div class="text-2xl font-black text-nationalDark">10+</div>
                            <div class="text-xs text-gray-500 font-semibold">Organisasi</div>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 card-shadow flex items-center space-x-4">
                        <div class="w-12 h-12 bg-red-100 text-nationalRed rounded-2xl flex items-center justify-center text-xl font-bold">🎬</div>
                        <div>
                            <div class="text-2xl font-black text-nationalDark">4</div>
                            <div class="text-xs text-gray-500 font-semibold">Video Edukasi</div>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 card-shadow flex items-center space-x-4">
                        <div class="w-12 h-12 bg-red-100 text-nationalRed rounded-2xl flex items-center justify-center text-xl font-bold">📝</div>
                        <div>
                            <div class="text-2xl font-black text-nationalDark">20</div>
                            <div class="text-xs text-gray-500 font-semibold">Soal Evaluasi</div>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 card-shadow flex items-center space-x-4">
                        <div class="w-12 h-12 bg-red-100 text-nationalRed rounded-2xl flex items-center justify-center text-xl font-bold">🎮</div>
                        <div>
                            <div class="text-2xl font-black text-nationalDark">1</div>
                            <div class="text-xs text-gray-500 font-semibold">Game Interaktif</div>
                        </div>
                    </div>
                </div>

                <!-- Quick Access Grid -->
                <div class="space-y-4">
                    <h3 class="text-xl font-black text-nationalDark">Mulai Belajar</h3>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="bg-white p-6 rounded-3xl border border-gray-200 card-shadow hover:border-nationalRed transition flex flex-col justify-between space-y-4">
                            <div>
                                <div class="text-3xl mb-2">⏳</div>
                                <h4 class="text-lg font-black text-nationalDark">Jejak Sejarah</h4>
                                <p class="text-gray-600 text-sm mt-1">“Telusuri perkembangan pergerakan nasional.”</p>
                            </div>
                            <button onclick="showPage('timeline')" class="self-start bg-nationalCreamCard hover:bg-red-50 hover:text-nationalRed text-nationalDark font-bold px-5 py-2.5 rounded-xl text-xs border border-gray-200 transition">
                                Jelajahi →
                            </button>
                        </div>
                        <div class="bg-white p-6 rounded-3xl border border-gray-200 card-shadow hover:border-nationalRed transition flex flex-col justify-between space-y-4">
                            <div>
                                <div class="text-3xl mb-2">🎬</div>
                                <h4 class="text-lg font-black text-nationalDark">Video Edukasi</h4>
                                <p class="text-gray-600 text-sm mt-1">“Belajar sejarah melalui visual & cerita.”</p>
                            </div>
                            <button onclick="showPage('videos')" class="self-start bg-nationalCreamCard hover:bg-red-50 hover:text-nationalRed text-nationalDark font-bold px-5 py-2.5 rounded-xl text-xs border border-gray-200 transition">
                                Tonton Video →
                            </button>
                        </div>
                        <div class="bg-white p-6 rounded-3xl border border-gray-200 card-shadow hover:border-nationalRed transition flex flex-col justify-between space-y-4">
                            <div>
                                <div class="text-3xl mb-2">📚</div>
                                <h4 class="text-lg font-black text-nationalDark">Materi Organisasi</h4>
                                <p class="text-gray-600 text-sm mt-1">“Pelajari organisasi dan tokoh penting.”</p>
                            </div>
                            <button onclick="showPage('materials')" class="self-start bg-nationalCreamCard hover:bg-red-50 hover:text-nationalRed text-nationalDark font-bold px-5 py-2.5 rounded-xl text-xs border border-gray-200 transition">
                                Buka Materi →
                            </button>
                        </div>
                        <div class="bg-white p-6 rounded-3xl border border-gray-200 card-shadow hover:border-nationalRed transition flex flex-col justify-between space-y-4">
                            <div>
                                <div class="text-3xl mb-2">🎮</div>
                                <h4 class="text-lg font-black text-nationalDark">Memory Match Game</h4>
                                <p class="text-gray-600 text-sm mt-1">“Uji ingatanmu melalui permainan.”</p>
                            </div>
                            <button onclick="showPage('game')" class="self-start bg-nationalCreamCard hover:bg-red-50 hover:text-nationalRed text-nationalDark font-bold px-5 py-2.5 rounded-xl text-xs border border-gray-200 transition">
                                Mainkan →
                            </button>
                        </div>
                        <div class="bg-white p-6 rounded-3xl border border-gray-200 card-shadow hover:border-nationalRed transition flex flex-col justify-between space-y-4">
                            <div>
                                <div class="text-3xl mb-2">📝</div>
                                <h4 class="text-lg font-black text-nationalDark">Evaluasi Akhir</h4>
                                <p class="text-gray-600 text-sm mt-1">“Uji pemahamanmu dengan 20 soal.”</p>
                            </div>
                            <button onclick="showPage('evaluation')" class="self-start bg-nationalCreamCard hover:bg-red-50 hover:text-nationalRed text-nationalDark font-bold px-5 py-2.5 rounded-xl text-xs border border-gray-200 transition">
                                Mulai Evaluasi →
                            </button>
                        </div>
                    </div>
                </div>
            </section>

            <!-- 2. TIMELINE PAGE -->
            <section id="page-timeline" class="page-section animate-fade-in space-y-6">
                <div>
                    <h2 class="text-2xl sm:text-3xl font-black text-nationalDark">Jejak Pergerakan Nasional</h2>
                    <p class="text-gray-600 text-sm mt-1">“Telusuri perjalanan organisasi dan peristiwa penting dalam perkembangan nasionalisme Indonesia.”</p>
                </div>

                <!-- Timeline Card Box -->
                <div class="bg-white p-6 sm:p-10 rounded-3xl border border-gray-200 card-shadow space-y-8">
                    <!-- Progress Dots -->
                    <div class="flex items-center justify-center space-x-1 sm:space-x-3 overflow-x-auto py-2" id="timeline-dots">
                        <!-- Injected via JS -->
                    </div>

                    <!-- Active Timeline Card Content -->
                    <div id="timeline-card-content" class="bg-nationalCream p-6 sm:p-8 rounded-2xl border border-gray-200 space-y-6 transition-all duration-300">
                        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                            <span id="tl-year" class="bg-nationalRed text-white text-xs font-black px-3.5 py-1.5 rounded-full uppercase tracking-wider w-max">1908</span>
                            <span id="tl-category" class="text-xs font-bold text-gray-500 uppercase tracking-widest">Era Perintisan</span>
                        </div>
                        
                        <div>
                            <h3 id="tl-title" class="text-2xl sm:text-3xl font-black text-nationalDark">Budi Utomo</h3>
                            <div class="mt-4 space-y-3">
                                <div>
                                    <h4 class="text-xs font-bold uppercase tracking-wider text-gray-400">Informasi Peristiwa:</h4>
                                    <p id="tl-info" class="text-gray-700 text-base leading-relaxed mt-1">Budi Utomo berdiri pada 20 Mei 1908 dan sering dipandang sebagai salah satu tonggak awal kebangkitan organisasi modern di Indonesia.</p>
                                </div>
                                <div class="pt-2">
                                    <h4 class="text-xs font-bold uppercase tracking-wider text-nationalRed">Tokoh Terkait:</h4>
                                    <p id="tl-tokoh" class="text-nationalDark font-bold text-sm mt-0.5">dr. Soetomo dan para pelajar STOVIA.</p>
                                </div>
                            </div>
                        </div>

                        <div class="pt-4 border-t border-gray-200 flex flex-col sm:flex-row justify-between items-center gap-4">
                            <span id="tl-counter" class="text-xs font-bold text-gray-500">1 / 8</span>
                            <button id="tl-learn-btn" onclick="openMaterialFromTimeline()" class="bg-nationalDark hover:bg-black text-white font-bold px-6 py-3 rounded-xl text-xs shadow transition flex items-center space-x-2">
                                <span>Pelajari Materi Organisasi</span>
                                <span>→</span>
                            </button>
                        </div>
                    </div>

                    <!-- Navigation Controls -->
                    <div class="flex justify-between items-center pt-2">
                        <button onclick="prevTimeline()" class="bg-nationalCream hover:bg-gray-200 text-nationalDark font-bold px-5 py-3 rounded-xl text-xs border border-gray-200 transition flex items-center space-x-2">
                            <span>← Sebelumnya</span>
                        </button>
                        <button onclick="nextTimeline()" class="bg-nationalRed hover:bg-nationalDarkRed text-white font-bold px-6 py-3 rounded-xl text-xs shadow transition flex items-center space-x-2">
                            <span>Berikutnya →</span>
                        </button>
                    </div>
                </div>
            </section>

            <!-- 3. VIDEO EDUKASI PAGE (NEW) -->
            <section id="page-videos" class="page-section animate-fade-in space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-2xl sm:text-3xl font-black text-nationalDark">🎬 Video Edukasi Pergerakan Nasional</h2>
                        <p class="text-gray-600 text-sm mt-1">“Pelajari Pergerakan Nasional Indonesia melalui video edukatif yang ringkas dan mudah dipahami.”</p>
                    </div>
                    <!-- Video Progress Badge -->
                    <div class="bg-white p-3 rounded-2xl border border-gray-200 card-shadow flex items-center space-x-3 w-max">
                        <div class="text-xs font-bold text-gray-500">Progress Video: <span id="video-progress-count" class="text-nationalRed font-black">0 / 0</span></div>
                        <div class="w-24 bg-gray-200 rounded-full h-2.5 overflow-hidden">
                            <div id="video-progress-bar" class="bg-nationalRed h-full transition-all duration-300" style="width: 0%"></div>
                        </div>
                    </div>
                </div>

                <!-- DAFTAR VIDEO YANG DIBUTUHKAN (CHECKLIST) -->
                <div class="bg-amber-50 border border-amber-200 p-6 rounded-3xl space-y-4">
                    <div class="flex items-center space-x-3">
                        <div class="text-2xl">📋</div>
                        <div>
                            <h3 class="font-black text-amber-900 text-base">Daftar File Video yang Perlu Disiapkan</h3>
                            <p class="text-xs text-amber-700">Simpan file video dengan format <code class="bg-white px-2 py-0.5 rounded font-bold text-nationalRed">.mp4</code> ke dalam folder <code class="bg-white px-2 py-0.5 rounded font-bold text-nationalRed">videos/</code> sesuai nama file di bawah ini:</p>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3" id="required-videos-checklist">
                        <!-- Injected via JS -->
                    </div>
                </div>

                <!-- Main Video Box -->
                <div class="bg-white p-6 sm:p-8 rounded-3xl border border-gray-200 card-shadow space-y-6">
                    <div class="relative w-full bg-black rounded-2xl overflow-hidden aspect-video shadow-lg flex items-center justify-center">
                        <!-- HTML5 Video Player with Offline Fallback -->
                        <video id="main-video-player" class="w-full h-full object-cover" controls onended="onVideoEnded()">
                            <source id="main-video-source" src="" type="video/mp4">
                            Browser Anda tidak mendukung pemutaran video HTML5.
                        </video>
                        <!-- Fallback overlay if video file missing -->
                        <div id="video-offline-fallback" class="absolute inset-0 bg-gray-900 text-white flex flex-col items-center justify-center p-6 text-center space-y-3 hidden">
                            <div class="text-4xl">⚠️</div>
                            <h4 class="text-lg font-bold">Video belum tersedia</h4>
                            <p class="text-xs text-gray-400 max-w-sm">File video belum ditemukan di folder <code class="bg-gray-800 text-red-400 px-2 py-0.5 rounded">videos/</code>. Pastikan file .mp4 sudah diletakkan sesuai nama file pada data.</p>
                            <label class="bg-nationalRed hover:bg-nationalDarkRed text-white text-xs font-bold px-4 py-2.5 rounded-xl cursor-pointer shadow transition">
                                Pilih File Video Lokal (.mp4)
                                <input type="file" accept="video/*" onchange="loadLocalVideoFile(this)" class="hidden">
                            </label>
                        </div>
                    </div>

                    <!-- Video Information Panel -->
                    <div class="space-y-4">
                        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                            <div class="flex items-center space-x-2">
                                <span id="main-vid-category" class="bg-red-100 text-nationalRed text-xs font-bold px-3 py-1 rounded-full">Pengantar</span>
                                <span id="main-vid-duration" class="text-xs font-semibold text-gray-500">⏱️ 00:00</span>
                                <span id="main-vid-status" class="text-xs font-bold px-2.5 py-1 rounded-full bg-gray-100 text-gray-600">Belum ditonton</span>
                            </div>
                            <button onclick="toggleWatchStatus()" id="btn-toggle-watch" class="bg-nationalCream hover:bg-gray-200 text-nationalDark font-bold px-4 py-2 rounded-xl text-xs border border-gray-200 transition">
                                Tandai Selesai ✓
                            </button>
                        </div>

                        <div>
                            <h3 id="main-vid-title" class="text-xl sm:text-2xl font-black text-nationalDark">Judul Video</h3>
                            <p id="main-vid-desc" class="text-gray-600 text-sm mt-2 leading-relaxed">Deskripsi video akan muncul di sini.</p>
                        </div>

                        <!-- Related Materials & Action -->
                        <div class="pt-4 border-t border-gray-100 flex flex-col sm:flex-row items-center justify-between gap-4">
                            <div class="flex items-center space-x-2 text-xs font-bold text-gray-500 w-full sm:w-auto">
                                <span>Materi terkait:</span>
                                <div id="main-vid-related" class="flex flex-wrap gap-1.5">
                                    <button onclick="showPage('materials')" class="bg-nationalCream hover:bg-red-50 hover:text-nationalRed text-nationalDark px-3 py-1.5 rounded-lg border border-gray-200 transition">Materi Organisasi</button>
                                </div>
                            </div>
                            <button id="btn-next-material" onclick="jumpToRelatedMaterial()" class="w-full sm:w-auto bg-nationalRed hover:bg-nationalDarkRed text-white font-bold px-6 py-3 rounded-xl text-xs shadow transition flex items-center justify-center space-x-2">
                                <span>Buka Materi Organisasi →</span>
                            </button>
                        </div>
                    </div>

                    <!-- Catatan Belajar (Notes Feature) -->
                    <div class="bg-nationalCream p-6 rounded-2xl border border-gray-200 space-y-3">
                        <div class="flex items-center justify-between">
                            <h4 class="text-sm font-black text-nationalDark flex items-center space-x-2">
                                <span>📝 Catatan Belajar</span>
                            </h4>
                            <span id="note-saved-status" class="text-[10px] text-green-600 font-bold hidden">Tersimpan di LocalStorage ✓</span>
                        </div>
                        <p class="text-xs text-gray-600">Apa yang kamu pelajari dari video ini? Tulis catatanmu di bawah:</p>
                        <textarea id="video-note-input" oninput="saveVideoNote()" placeholder="Tulis ringkasan atau poin penting video di sini..." class="w-full h-24 p-3 bg-white border border-gray-200 rounded-xl text-xs font-medium focus:outline-none focus:border-nationalRed"></textarea>
                    </div>
                </div>

                <!-- Video Playlist & Filters Section -->
                <div class="space-y-4">
                    <div class="flex flex-col md:flex-row gap-4 items-center justify-between bg-white p-4 rounded-2xl border border-gray-200 card-shadow">
                        <div class="relative w-full md:w-80">
                            <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-400">🔍</span>
                            <input type="text" id="search-video" oninput="filterVideos()" placeholder="Cari judul atau deskripsi..." class="w-full pl-10 pr-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-xs font-semibold focus:outline-none focus:border-nationalRed">
                        </div>
                        <div class="flex flex-wrap gap-1.5 w-full md:w-auto" id="video-filter-buttons">
                            <button onclick="setVideoFilter('Semua')" class="v-filter-btn active-v-filter px-3.5 py-1.5 rounded-xl text-xs font-bold bg-nationalRed text-white transition">Semua</button>
                            <button onclick="setVideoFilter('Pengantar')" class="v-filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Pengantar</button>
                            <button onclick="setVideoFilter('Organisasi')" class="v-filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Organisasi</button>
                            <button onclick="setVideoFilter('Tokoh')" class="v-filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Tokoh</button>
                            <button onclick="setVideoFilter('Peristiwa')" class="v-filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Peristiwa</button>
                        </div>
                    </div>

                    <!-- Video Cards Grid -->
                    <div id="video-playlist-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                        <!-- Injected via JS -->
                    </div>
                </div>
            </section>

            <!-- 4. MATERIALS PAGE -->
            <section id="page-materials" class="page-section animate-fade-in space-y-6">
                <div>
                    <h2 class="text-2xl sm:text-3xl font-black text-nationalDark">Materi Organisasi Pergerakan</h2>
                    <p class="text-gray-600 text-sm mt-1">“Pelajari karakteristik, asas, dan peran setiap organisasi dalam pergerakan nasional.”</p>
                </div>

                <!-- Search & Filters -->
                <div class="flex flex-col md:flex-row gap-4 items-center justify-between bg-white p-4 rounded-2xl border border-gray-200 card-shadow">
                    <div class="relative w-full md:w-80">
                        <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-400">🔍</span>
                        <input type="text" id="search-org" oninput="filterOrganizations()" placeholder="Cari organisasi atau tokoh..." class="w-full pl-10 pr-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-xs font-semibold focus:outline-none focus:border-nationalRed">
                    </div>
                    <div class="flex flex-wrap gap-1.5 w-full md:w-auto" id="filter-buttons">
                        <button onclick="setFilter('Semua')" class="filter-btn active-filter px-3.5 py-1.5 rounded-xl text-xs font-bold bg-nationalRed text-white transition">Semua</button>
                        <button onclick="setFilter('Politik')" class="filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Politik</button>
                        <button onclick="setFilter('Sosial & Agama')" class="filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Sosial & Agama</button>
                        <button onclick="setFilter('Pendidikan')" class="filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Pendidikan</button>
                        <button onclick="setFilter('Pemuda')" class="filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition">Pemuda</button>
                    </div>
                </div>

                <!-- Organization Cards Grid -->
                <div id="org-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Injected via JS -->
                </div>
            </section>

            <!-- 5. GAME PAGE -->
            <section id="page-game" class="page-section animate-fade-in space-y-6">
                <div>
                    <h2 class="text-2xl sm:text-3xl font-black text-nationalDark">🎮 Memory Match: Pasangkan Organisasi</h2>
                    <p class="text-gray-600 text-sm mt-1">“Uji ingatanmu dengan mencocokkan kartu nama organisasi dengan tokoh utamanya.”</p>
                </div>

                <!-- Game Container -->
                <div class="bg-white p-6 sm:p-8 rounded-3xl border border-gray-200 card-shadow space-y-6">
                    <!-- Scoreboard -->
                    <div class="flex flex-col sm:flex-row justify-between items-center bg-nationalCream p-4 rounded-2xl border border-gray-200 gap-4 text-xs sm:text-sm font-bold">
                        <div class="flex items-center space-x-6">
                            <div>Skor: <span id="game-score" class="text-nationalRed text-base">0</span> / 6</div>
                            <div>Percobaan: <span id="game-tries" class="text-nationalDark text-base">0</span></div>
                            <div>Waktu: <span id="game-timer" class="text-nationalDark text-base">00:00</span></div>
                        </div>
                        <button onclick="startMemoryGame()" class="bg-nationalRed hover:bg-nationalDarkRed text-white font-bold px-5 py-2.5 rounded-xl shadow transition">
                            🔄 Mulai / Ulangi Game
                        </button>
                    </div>

                    <!-- Cards Board -->
                    <div id="memory-board" class="grid grid-cols-3 sm:grid-cols-4 gap-3 sm:gap-4 min-h-[320px] items-center justify-center">
                        <div class="col-span-full text-center py-20 text-gray-400 font-semibold text-sm">
                            Klik tombol <strong>Mulai / Ulangi Game</strong> di atas untuk memulai permainan!
                        </div>
                    </div>
                </div>
            </section>

            <!-- 6. EVALUASI PAGE -->
            <section id="page-evaluation" class="page-section animate-fade-in space-y-6">
                <div>
                    <h2 class="text-2xl sm:text-3xl font-black text-nationalDark">📝 Evaluasi Akhir Siswa SMA</h2>
                    <p class="text-gray-600 text-sm mt-1">“Uji pemahaman menyeluruhmu melalui 20 soal pilihan ganda.”</p>
                </div>

                <!-- Quiz Container -->
                <div id="quiz-box" class="bg-white p-6 sm:p-10 rounded-3xl border border-gray-200 card-shadow space-y-6">
                    <!-- Progress Bar Quiz -->
                    <div class="flex justify-between items-center text-xs font-bold text-gray-500 border-b border-gray-100 pb-4">
                        <span id="quiz-progress-label">Soal 1 dari 20</span>
                        <span class="text-nationalRed">Bobot: 5 Poin / Soal</span>
                    </div>

                    <!-- Question Card -->
                    <div class="space-y-4">
                        <h3 id="quiz-question-text" class="text-lg sm:text-xl font-bold text-nationalDark">
                            Pertanyaan akan dimuat...
                        </h3>
                        <div id="quiz-options-list" class="space-y-3 pt-2">
                            <!-- Options injected via JS -->
                        </div>
                    </div>

                    <!-- Feedback & Navigation -->
                    <div class="pt-6 border-t border-gray-100 flex flex-col sm:flex-row justify-between items-center gap-4">
                        <div id="quiz-feedback" class="text-sm font-bold"></div>
                        <button id="quiz-submit-btn" onclick="submitQuizAnswer()" class="w-full sm:w-auto bg-nationalRed hover:bg-nationalDarkRed text-white font-bold px-8 py-3.5 rounded-xl shadow transition text-xs">
                            Jawab & Lanjut →
                        </button>
                    </div>
                </div>

                <!-- Results Box (Hidden Initially) -->
                <div id="quiz-results-box" class="hidden bg-white p-8 sm:p-10 rounded-3xl border border-gray-200 card-shadow text-center space-y-6 animate-fade-in">
                    <!-- Populated via JS -->
                </div>
            </section>

            <!-- 7. ABOUT PAGE -->
            <section id="page-about" class="page-section animate-fade-in space-y-6">
                <div>
                    <h2 class="text-2xl sm:text-3xl font-black text-nationalDark">Tentang EsHist</h2>
                    <p class="text-gray-600 text-sm mt-1">Informasi latar belakang aplikasi pembelajaran sejarah interaktif.</p>
                </div>

                <div class="bg-white p-8 sm:p-10 rounded-3xl border border-gray-200 card-shadow space-y-6">
                    <div class="flex items-center space-x-4">
                        <div class="w-14 h-14 bg-nationalRed rounded-2xl flex items-center justify-center text-white font-black text-2xl shadow-md">
                            📚
                        </div>
                        <div>
                            <h3 class="text-2xl font-black text-nationalDark">Es<span class="text-nationalRed">Hist</span></h3>
                            <span class="text-xs text-gray-500 font-bold uppercase tracking-wider">Eksplorasi Sejarah Interaktif</span>
                        </div>
                    </div>
                    
                    <div class="space-y-4 text-sm text-gray-700 leading-relaxed border-t border-gray-100 pt-6">
                        <p>
                            <strong>EsHist</strong> merupakan media pembelajaran sejarah interaktif yang dirancang khusus untuk membantu siswa Sekolah Menengah Atas (SMA) memahami Pergerakan Nasional Indonesia secara mendalam dan menyenangkan.
                        </p>
                        <div class="bg-nationalCream p-5 rounded-2xl border border-gray-200 space-y-2">
                            <h4 class="font-bold text-nationalDark">🎯 Tujuan Utama Pembelajaran:</h4>
                            <p class="text-gray-600">
                                Membuat pembelajaran sejarah lebih interaktif, mudah dipahami, dan mendorong siswa untuk aktif mengeksplorasi materi organisasi, video edukasi, peristiwa, serta tokoh-tokoh penting tanpa ketergantungan pada koneksi internet.
                            </p>
                        </div>
                    </div>

                    <div class="pt-4 text-xs text-gray-400 border-t border-gray-100 flex justify-between items-center">
                        <span>EsHist | Eksplorasi Sejarah Interaktif</span>
                        <span>&copy; 2026 EsHist</span>
                    </div>
                </div>
            </section>

        </div>
    </main>

    <!-- Organization Detail Modal -->
    <div id="org-modal" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm hidden items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-2xl w-full p-6 sm:p-8 shadow-2xl relative max-h-[90vh] overflow-y-auto animate-fade-in">
            <button onclick="closeOrgModal()" class="absolute top-6 right-6 w-10 h-10 bg-gray-100 hover:bg-red-100 text-gray-500 hover:text-nationalRed rounded-full flex items-center justify-center font-bold transition">
                ✕
            </button>
            <div id="modal-content" class="space-y-6">
                <!-- Dynamically populated -->
            </div>
        </div>
    </div>

    <!-- Game Win Modal -->
    <div id="game-win-modal" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm hidden items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-8 shadow-2xl text-center space-y-6 animate-fade-in">
            <div class="w-16 h-16 bg-green-500 text-white rounded-full flex items-center justify-center mx-auto text-3xl shadow-md">
                🏆
            </div>
            <div>
                <h3 class="text-2xl font-black text-nationalDark">Permainan Selesai!</h3>
                <p id="game-win-stats" class="text-sm text-gray-600 mt-2">Semua kartu berhasil dipasangkan dengan luar biasa.</p>
            </div>
            <div class="flex flex-col sm:flex-row gap-3 pt-2">
                <button onclick="closeGameModal(); startMemoryGame();" class="flex-1 bg-nationalCream hover:bg-gray-200 text-nationalDark font-bold py-3 px-4 rounded-xl text-xs border border-gray-200 transition">
                    Coba Lagi
                </button>
                <button onclick="closeGameModal(); showPage('evaluation');" class="flex-1 bg-nationalRed hover:bg-nationalDarkRed text-white font-bold py-3 px-4 rounded-xl text-xs shadow transition">
                    Lanjut ke Evaluasi →
                </button>
            </div>
        </div>
    </div>

    <script>
        // --- DATA REPOSITORY ---

        const timelineData = [
            {
                year: "1908",
                category: "Era Perintisan",
                title: "Budi Utomo",
                info: "Budi Utomo berdiri pada 20 Mei 1908 dan sering dipandang sebagai salah satu tonggak awal kebangkitan organisasi modern di Indonesia.",
                tokoh: "dr. Soetomo dan para pelajar STOVIA.",
                orgId: "budi-utomo"
            },
            {
                year: "1911–1912",
                category: "Era Perdagangan & Massa",
                title: "Sarekat Islam",
                info: "Sarekat Islam berkembang dari organisasi perdagangan menjadi organisasi massa yang memiliki pengaruh luas dalam kehidupan sosial dan politik masyarakat.",
                tokoh: "H.O.S. Tjokroaminoto.",
                orgId: "sarekat-islam"
            },
            {
                year: "1912",
                category: "Partai Politik Pertama",
                title: "Indische Partij",
                info: "Indische Partij didirikan oleh Tiga Serangkai dan menyuarakan gagasan persatuan penduduk Hindia serta perjuangan politik terhadap kolonialisme.",
                tokoh: "Douwes Dekker, Ki Hadjar Dewantara, dan dr. Cipto Mangunkusumo.",
                orgId: "indische-partij"
            },
            {
                year: "1912",
                category: "Gerakan Sosial & Keagamaan",
                title: "Muhammadiyah",
                info: "Muhammadiyah didirikan di Yogyakarta dan bergerak terutama dalam bidang pendidikan, sosial, dan keagamaan.",
                tokoh: "K.H. Ahmad Dahlan.",
                orgId: "muhammadiyah"
            },
            {
                year: "1922",
                category: "Pendidikan Nasional",
                title: "Taman Siswa",
                info: "Taman Siswa didirikan sebagai gerakan pendidikan yang mengembangkan pendidikan bagi masyarakat pribumi serta menekankan kemandirian dan kebudayaan.",
                tokoh: "Ki Hadjar Dewantara.",
                orgId: "taman-siswa"
            },
            {
                year: "1925",
                category: "Pergerakan Luar Negeri",
                title: "Perhimpunan Indonesia",
                info: "Perhimpunan Indonesia berkembang di Belanda dan menjadi salah satu organisasi yang secara terbuka memperjuangkan gagasan kemerdekaan Indonesia.",
                tokoh: "Mohammad Hatta dan tokoh mahasiswa Indonesia lainnya.",
                orgId: "perhimpunan-indonesia"
            },
            {
                year: "1927",
                category: "Partai Nasionalis",
                title: "Partai Nasional Indonesia (PNI)",
                info: "PNI didirikan di Bandung dan memperjuangkan kemerdekaan Indonesia melalui gagasan nasionalisme dan perjuangan politik.",
                tokoh: "Ir. Soekarno.",
                orgId: "pni"
            },
            {
                year: "1928",
                category: "Puncak Persatuan",
                title: "Sumpah Pemuda",
                info: "Kongres Pemuda II menghasilkan ikrar yang menegaskan satu tanah air, satu bangsa, dan satu bahasa, yaitu Indonesia.",
                tokoh: "Para pemuda dari berbagai organisasi kepemudaan.",
                orgId: "sumpah-pemuda"
            }
        ];

        // ==========================================
        // TAMBAHKAN VIDEO BARU DI SINI
        // ==========================================
        const videos = [
            {
                id: 1,
                title: "Mengenal Pergerakan Nasional Indonesia",
                category: "Pengantar",
                duration: "07:04",
                file: "https://youtu.be/-WdlE8EjR80?si=5NaYWj75Ezh4t_FP",
                description: "Pengantar komprehensif mengenai latar belakang penjajahan kolonial, penderitaan rakyat, dan awal kesadaran nasional untuk merebut kemerdekaan."
            },
            {
                id: 2,
                title: "Budi Utomo dan Awal Kebangkitan Nasional",
                category: "Organisasi",
                duration: "06:57",
                file: "https://youtu.be/BsncD-RLDJ0?si=3RrWCfLgtgqTS5FG",
                description: "Membahas secara mendalam berdirinya Budi Utomo pada 20 Mei 1908 oleh dr. Soetomo dan para pelajar STOVIA sebagai tonggak kesadaran berorganisasi."
            },
            {
                id: 3,
                title: "Organisasi Pergerakan Nasional",
                category: "Organisasi",
                duration: "06:10",
                file: "https://youtu.be/BGMCaCwEdTE?si=yar1pv-3xlu-WRdd",
                description: "Eksplorasi berbagai organisasi besar mulai dari Sarekat Islam, Indische Partij, hingga pergerakan radikal dan moderat di Hindia Belanda."
            },
            {
                id: 4,
                title: "Sumpah Pemuda 1928",
                category: "Peristiwa",
                duration: "02:52",
                file: "https://youtu.be/DKbAb08tB30?si=hbUdVdKJue2TB3ns",
                description: "Momen bersejarah Kongres Pemuda II yang melahirkan ikrar satu tanah air, satu bangsa, dan satu bahasa persatuan Indonesia."
            },
            {
                id: 5,
                title: "Sarekat Islam",
                category: "Organisasi",
                duration: "04:44",
                file: "https://youtu.be/lHdteI0j7JI?si=Y5025FuviEhW5VLv",
                description: "Mengenal perkembangan Sarekat Islam dalam Pergerakan Nasional Indonesia."
            }
        ];

        const organizationsData = [
            {
                id: "budi-utomo",
                name: "Budi Utomo",
                year: "1908",
                category: "Pendidikan",
                founder: "dr. Soetomo & para pelajar STOVIA",
                focus: "Pendidikan, kebudayaan, dan sosial masyarakat.",
                background: "Didirikan di Jakarta atas inspirasi dr. Wahidin Sudirohusodo untuk mengumpulkan dana pelajar guna membantu kaum muda pribumi yang cerdas.",
                purpose: "Memajukan pengajaran, pertanian, perdagangan, teknik, industri, dan kebudayaan.",
                role: "Menjadi pelopor berdirinya organisasi modern pertama di Indonesia yang membangkitkan kesadaran berorganisasi.",
                fact: "Tanggal lahir Budi Utomo (20 Mei) diperingati sebagai Hari Kebangkitan Nasional."
            },
            {
                id: "sarekat-islam",
                name: "Sarekat Islam (SI)",
                year: "1911–1912",
                category: "Politik",
                founder: "H. Samanhudi & H.O.S. Tjokroaminoto",
                focus: "Perdagangan, ekonomi kerakyatan, dan pembelaan hak rakyat.",
                background: "Bermula dari Sarekat Dagang Islam di Solo untuk melindungi pedagang batik pribumi dari persaingan asing.",
                purpose: "Menumbuhkan jiwa berniaga dan menentang penindasan kolonial.",
                role: "Menjadi organisasi massa pertama yang berhasil menghimpun jutaan anggota dari berbagai lapisan.",
                fact: "H.O.S. Tjokroaminoto dikenal sebagai 'Raja Tanpa Mahkota' yang melahirkan banyak tokoh besar."
            },
            {
                id: "indische-partij",
                name: "Indische Partij",
                year: "1912",
                category: "Politik",
                founder: "Douwes Dekker, Tjipto Mangoenkoesoemo, Ki Hadjar Dewantara",
                focus: "Politik nasionalis radikal dan kemerdekaan Hindia.",
                background: "Didirikan oleh Tiga Serangkai sebagai partai politik pertama di Hindia Belanda yang secara terbuka menuntut kemerdekaan.",
                purpose: "Membangkitkan rasa cinta tanah air bagi seluruh penduduk tanpa memandang ras.",
                role: "Mengubah haluan perjuangan dari kooperatif menjadi politik radikal anti-kolonial secara terang-terangan.",
                fact: "Para pendirinya diasingkan ke negeri Belanda akibat tulisan kritikan tajam terhadap pemerintah kolonial."
            },
            {
                id: "muhammadiyah",
                name: "Muhammadiyah",
                year: "1912",
                category: "Sosial & Agama",
                founder: "K.H. Ahmad Dahlan",
                focus: "Pendidikan modern, sosial, dan pemurnian keagamaan.",
                background: "Didirikan di Kampung Kauman Yogyakarta untuk memperbaiki pemahaman Islam dan mengatasi keterbelakangan pendidikan.",
                purpose: "Menyebarkan ajaran Islam berdasarkan Al-Qur'an dan Sunnah serta mendirikan amal usaha.",
                role: "Memelopori sistem pendidikan modern berbasis agama dan sains serta layanan sosial kemanusiaan.",
                fact: "Muhammadiyah membangun sekolah-sekolah umum bercorak Islam pertama di Indonesia."
            },
            {
                id: "taman-siswa",
                name: "Taman Siswa",
                year: "1922",
                category: "Pendidikan",
                founder: "Ki Hadjar Dewantara",
                focus: "Pendidikan nasional kerakyatan dan kebudayaan.",
                background: "Didirikan sebagai bentuk perlawanan kultural terhadap sistem pendidikan kolonial Belanda yang diskriminatif.",
                purpose: "Membangun kemerdekaan jiwa anak didik serta melestarikan kebudayaan nasional.",
                role: "Menyediakan akses pendidikan bagi anak-anak pribumi dari berbagai kalangan.",
                fact: "Semboyan pendidikan 'Tut Wuri Handayani' lahir dari perguruan ini."
            },
            {
                id: "perhimpunan-indonesia",
                name: "Perhimpunan Indonesia",
                year: "1925",
                category: "Politik",
                founder: "Mohammad Hatta, Ali Sastroamidjojo, dll.",
                focus: "Perjuangan politik luar negeri dan propaganda kemerdekaan.",
                background: "Transformasi dari Indische Vereeniging di Belanda menjadi wadah politik radikal mahasiswa Indonesia di Eropa.",
                purpose: "Menuntut kemerdekaan penuh bagi Indonesia dan menyuarakan aspirasi di forum internasional.",
                role: "Menjadi jembatan diplomasi internasional dan melahirkan kader pemimpin nasional.",
                fact: "Mohammad Hatta menyampaikan pidato pembelaan heroik berjudul 'Indonesia Merdeka' di pengadilan Den Haag."
            },
            {
                id: "pni",
                name: "Partai Nasional Indonesia (PNI)",
                year: "1927",
                category: "Politik",
                founder: "Ir. Soekarno dan kawan-kawan",
                focus: "Nasionalisme politik radikal non-kooperatif.",
                background: "Dibentuk di Bandung untuk menyatukan seluruh tenaga pergerakan nasional dalam satu barisan.",
                purpose: "Mencapai Indonesia Merdeka dengan asas percaya pada kekuatan sendiri (self-help).",
                role: "Mengobarkan semangat nasionalisme massa secara gigih melalui pidato-pidato berapi-api.",
                fact: "Ir. Soekarno menyusun pledoi pembelaan terkenal berjudul 'Indonesia Menggugat' di Landraad Bandung."
            },
            {
                id: "jong-java",
                name: "Jong Java",
                year: "1915",
                category: "Pemuda",
                founder: "Satiman Wirjosandjojo",
                focus: "Persatuan pemuda pelajar Jawa, Madura, Bali, dan Lombok.",
                background: "Semula bernama Tri Dharma, organisasi ini mewadahi para pelajar pribumi tingkat menengah.",
                purpose: "Memperkuat rasa persatuan di antara pemuda daerah menuju persatuan Indonesia raya.",
                role: "Menjadi pelopor utama yang meleburkan diri dalam wadah persatuan pemuda nasional.",
                fact: "Organisasi kepemudaan kedaerahan ini bertransformasi mendukung penuh persatuan nasional."
            },
            {
                id: "jong-sumatranen-bond",
                name: "Jong Sumatranen Bond",
                year: "1917",
                category: "Pemuda",
                founder: "Mohammad Hatta, Sanusi Pane, dll.",
                focus: "Persatuan pemuda pelajar asal Sumatera.",
                background: "Perkumpulan pemuda pelajar asal Sumatera yang bersekolah di Batavia.",
                purpose: "Mempererat hubungan antar pelajar dan mendidik calon pemimpin bangsa.",
                role: "Melahirkan tokoh-tokoh besar yang aktif merumuskan Sumpah Pemuda.",
                fact: "Mohammad Yamin dari organisasi ini berperan penting merumuskan naskah Sumpah Pemuda 1928."
            },
            {
                id: "nahdlatul-ulama",
                name: "Nahdlatul Ulama (NU)",
                year: "1926",
                category: "Sosial & Agama",
                founder: "K.H. Hasyim Asy'ari, K.H. Wahab Chasbullah",
                focus: "Keagamaan, sosial, dan pendidikan pesantren.",
                background: "Didirikan di Surabaya oleh para ulama pesantren untuk mempertahankan ajaran Ahlussunnah wal Jama'ah.",
                purpose: "Memelihara ajaran Islam tradisional, memajukan madrasah, serta membela kaum tani.",
                role: "Menggerakkan kesadaran kebangsaan yang memadukan cinta tanah air dengan keimanan.",
                fact: "Resolusi Jihad tahun 1945 menggerakkan perlawanan rakyat mempertahankan kemerdekaan."
            }
        ];

        const questionsData = [
            {
                q: "Organisasi Budi Utomo yang didirikan pada 20 Mei 1908 dicetuskan pertama kali oleh para pelajar sekolah kedokteran...",
                options: ["A. OSVIA", "B. STOVIA", "C. AMS", "D. HBS"],
                answer: 1
            },
            {
                q: "Tokoh utama yang menjadi inspirator berdirinya Budi Utomo melalui gagasan bantuan dana pelajar adalah...",
                options: ["A. dr. Wahidin Sudirohusodo", "B. H. Samanhudi", "C. H.O.S. Tjokroaminoto", "D. Douwes Dekker"],
                answer: 0
            },
            {
                q: "Sarekat Islam (SI) yang berkembang pesat bermula dari perkumpulan Sarekat Dagang Islam yang didirikan di kota...",
                options: ["A. Jakarta", "B. Surabaya", "C. Surakarta (Solo)", "D. Bandung"],
                answer: 2
            },
            {
                q: "Tiga Serangkai pendiri Indische Partij (1912) terdiri atas Douwes Dekker, Tjipto Mangoenkoesoemo, dan...",
                options: ["A. Ki Hadjar Dewantara", "B. Mohammad Hatta", "C. Ir. Soekarno", "D. dr. Soetomo"],
                answer: 0
            },
            {
                q: "Muhammadiyah didirikan di Yogyakarta pada tahun 1912 oleh seorang ulama pembaharu bernama...",
                options: ["A. K.H. Hasyim Asy'ari", "B. K.H. Ahmad Dahlan", "C. K.H. Mas Mansur", "D. K.H. Wahid Hasyim"],
                answer: 1
            },
            {
                q: "Perguruan Taman Siswa yang mengusung asas Among didirikan pada tahun 1922 oleh...",
                options: ["A. Ki Hadjar Dewantara", "B. Douwes Dekker", "C. Mohammad Yamin", "D. Soegondo Djojopoespito"],
                answer: 0
            },
            {
                q: "Perhimpunan Indonesia merupakan organisasi mahasiswa dan pelajar Indonesia yang berkembang di negara...",
                options: ["A. Jerman", "B. Belanda", "C. Prancis", "D. Belgia"],
                answer: 1
            },
            {
                q: "Partai Nasional Indonesia (PNI) didirikan di Bandung pada tahun 1927 dengan tokoh utama ketuanya yaitu...",
                options: ["A. Mohammad Hatta", "B. Sutan Sjahrir", "C. Ir. Soekarno", "D. Amir Sjarifuddin"],
                answer: 2
            },
            {
                q: "Kongres Pemuda II yang melahirkan ikrar Sumpah Pemuda diselenggarakan pada tanggal 27-28 Oktober tahun...",
                options: ["A. 1908", "B. 1918", "C. 1928", "D. 1945"],
                answer: 2
            },
            {
                q: "Tokoh pemuda yang memimpin jalannya Kongres Pemuda II dan mengetok palu keputusan Sumpah Pemuda adalah...",
                options: ["A. Soegondo Djojopoespito", "B. Mohammad Yamin", "C. W.R. Soepratman", "D. Sunario"],
                answer: 0
            },
            {
                q: "Lagu kebangsaan Indonesia Raya untuk pertama kalinya diperdengarkan secara instrumental oleh penciptanya pada...",
                options: ["A. Kongres Pemuda I", "B. Kongres Pemuda II", "C. Proklamasi Kemerdekaan", "D. Sidang BPUPKI"],
                answer: 1
            },
            {
                q: "Nahdlatul Ulama (NU) didirikan di Surabaya pada tahun 1926 oleh para ulama pesantren di bawah pimpinan...",
                options: ["A. K.H. Hasyim Asy'ari", "B. K.H. Ahmad Dahlan", "C. K.H. Mas Mansur", "D. K.H. Wahid Hasyim"],
                answer: 0
            },
            {
                q: "Faktor internal yang mendorong lahirnya Pergerakan Nasional Indonesia adalah...",
                options: ["A. Kemenangan Jepang atas Rusia", "B. Lahirnya kaum terpelajar pribumi", "C. Pergerakan nasional India", "D. Politik Etis kolonial semata"],
                answer: 1
            },
            {
                q: "Politik Etis (Politik Balas Budi) yang diterapkan Belanda mencakup tiga program utama, kecuali...",
                options: ["A. Irigasi", "B. Emigrasi", "C. Edukasi", "D. Industrialisasi militer"],
                answer: 3
            },
            {
                q: "Organisasi kepemudaan Jong Sumatranen Bond melahirkan tokoh besar yang kemudian menjadi Wakil Presiden pertama RI yaitu...",
                options: ["A. Adam Malik", "B. Mohammad Hatta", "C. Amir Sjarifuddin", "D. Tan Malaka"],
                answer: 1
            },
            {
                q: "Semboyan 'Self-help' (menolong diri sendiri) dan non-kooperatif menjadi asas perjuangan dari partai...",
                options: ["A. Budi Utomo", "B. Parindra", "C. PNI", "D. Gerindo"],
                answer: 2
            },
            {
                q: "Pledoi atau pembelaan terkenal yang disampaikan Ir. Soekarno di depan pengadilan kolonial Bandung berjudul...",
                options: ["A. Indonesia Menggugat", "B. Menuju Republik Indonesia", "C. Mentjari Indonesia", "D. Dari Djawa Menuju Indonesia"],
                answer: 0
            },
            {
                q: "Organisasi Indische Partij bersifat nasionalis radikal karena secara terang-terangan menuntut...",
                options: ["A. Perbaikan sekolah desa", "B. Kemerdekaan Hindia dari Belanda", "C. Penurunan pajak perdagangan", "D. Hak suara bangsawan"],
                answer: 1
            },
            {
                q: "Tanggal 20 Mei yang merupakan hari lahir Budi Utomo diperingati bangsa Indonesia sebagai hari...",
                options: ["A. Sumpah Pemuda", "B. Pahlawan", "C. Kebangkitan Nasional", "D. Pendidikan Nasional"],
                answer: 2
            },
            {
                q: "Makna utama dari peristiwa Sumpah Pemuda 1928 bagi perjuangan bangsa adalah...",
                options: ["A. Penghapusan batas-batas pulau", "B. Penyatuan tekad berbangsa dan bertanah air satu melampaui ikatan kedaerahan", "C. Pembentukan kabinet pertama", "D. Pengusiran seluruh bangsa asing"],
                answer: 1
            }
        ];

        const memoryPairs = [
            { org: "Budi Utomo", match: "dr. Soetomo" },
            { org: "Muhammadiyah", match: "K.H. Ahmad Dahlan" },
            { org: "Indische Partij", match: "Douwes Dekker" },
            { org: "PNI", match: "Ir. Soekarno" },
            { org: "Taman Siswa", match: "Ki Hadjar Dewantara" },
            { org: "Perhimpunan Indonesia", match: "Mohammad Hatta" }
        ];

        // --- STATE MANAGEMENT & LOCALSTORAGE ---

        let userProgress = {
            home: true,
            timeline: false,
            videos: false,
            materials: false,
            game: false,
            evaluation: false
        };

        let watchedVideos = {}; // vId -> boolean
        let videoNotes = {};    // vId -> text
        let currentVideoId = 1;
        let currentVideoFilter = "Semua";

        let currentTimelineIdx = 0;
        let activeFilter = "Semua";

        // Quiz State
        let currentQuizIdx = 0;
        let userAnswers = {};

        // Game State
        let gameCards = [];
        let flippedCards = [];
        let matchedPairs = 0;
        let gameTries = 0;
        let gameTimer = 0;
        let gameInterval = null;
        let isGameLocked = false;

        function loadProgress() {
            const savedProg = localStorage.getItem('eshist_progress_v3');
            if (savedProg) {
                try { userProgress = JSON.parse(savedProg); } catch(e){}
            }
            const savedWatch = localStorage.getItem('eshist_watched_videos');
            if (savedWatch) {
                try { watchedVideos = JSON.parse(savedWatch); } catch(e){}
            }
            const savedNotes = localStorage.getItem('eshist_video_notes');
            if (savedNotes) {
                try { videoNotes = JSON.parse(savedNotes); } catch(e){}
            }
            updateProgressUI();
        }

        function markPageVisited(pageKey) {
            userProgress[pageKey] = true;
            localStorage.setItem('eshist_progress_v3', JSON.stringify(userProgress));
            updateProgressUI();
        }

        function updateProgressUI() {
            const keys = Object.keys(userProgress);
            const visitedCount = keys.filter(k => userProgress[k]).length;
            const pct = Math.round((visitedCount / keys.length) * 100);

            document.getElementById('topbar-progress-text').innerText = pct + '%';
            document.getElementById('topbar-progress-bar').style.width = pct + '%';

            // Also update video progress widget
            const totalV = videos.length;
            const watchedCount = Object.keys(watchedVideos).filter(id => watchedVideos[id]).length;
            document.getElementById('video-progress-count').innerText = `${watchedCount} / ${totalV}`;
            const vPct = totalV > 0 ? Math.round((watchedCount / totalV) * 100) : 0;
            document.getElementById('video-progress-bar').style.width = vPct + '%';

            if (watchedCount > 0) {
                userProgress['videos'] = true;
                localStorage.setItem('eshist_progress_v3', JSON.stringify(userProgress));
            }
        }

        // --- NAVIGATION SYSTEM ---

        const pageTitles = {
            home: "Beranda",
            timeline: "Jejak Sejarah",
            videos: "Video Edukasi",
            materials: "Materi Organisasi",
            game: "Memory Match",
            evaluation: "Evaluasi Siswa",
            about: "Tentang EsHist"
        };

        function showPage(pageId) {
            // Hide all sections
            document.querySelectorAll('.page-section').forEach(sec => {
                sec.classList.remove('active');
            });

            // Show selected section
            const target = document.getElementById(`page-${pageId}`);
            if (target) {
                target.classList.add('active');
            }

            // Update Topbar Title
            document.getElementById('topbar-title').innerText = pageTitles[pageId] || "Dashboard";

            // Update Desktop Sidebar active state
            document.querySelectorAll('.sidebar-item').forEach(btn => btn.classList.remove('active'));
            const desktopNavBtn = document.getElementById(`nav-${pageId}`);
            if (desktopNavBtn) desktopNavBtn.classList.add('active');

            // Update Mobile Bottom Nav active state
            document.querySelectorAll('nav.md\\:hidden button').forEach(btn => {
                btn.classList.remove('text-nationalRed');
                btn.classList.add('text-gray-500');
            });
            const mobBtn = document.getElementById(`mob-${pageId}`);
            if (mobBtn) {
                mobBtn.classList.remove('text-gray-500');
                mobBtn.classList.add('text-nationalRed');
            }

            markPageVisited(pageId);
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // --- VIDEO EDUKASI CONTROLLER ---

        function renderVideoPlayer(vId) {
            const v = videos.find(item => item.id === vId);
            if (!v) return;
            currentVideoId = vId;

            // Update Player Sources
            const player = document.getElementById('main-video-player');
            const source = document.getElementById('main-video-source');
            const fallback = document.getElementById('video-offline-fallback');

            source.src = v.file;
            player.load();

            // Handle missing local file gracefully with error event
            player.onerror = function() {
                fallback.classList.remove('hidden');
                player.classList.add('hidden');
            };
            player.oncanplay = function() {
                fallback.classList.add('hidden');
                player.classList.remove('hidden');
            };

            // Update Information Panel
            document.getElementById('main-vid-category').innerText = v.category;
            document.getElementById('main-vid-duration').innerText = `⏱️ ${v.duration}`;
            document.getElementById('main-vid-title').innerText = v.title;
            document.getElementById('main-vid-desc').innerText = v.description;

            // Watch status badge
            const isWatched = watchedVideos[v.id] || false;
            const statusBadge = document.getElementById('main-vid-status');
            const toggleBtn = document.getElementById('btn-toggle-watch');
            if (isWatched) {
                statusBadge.innerText = "✓ Sudah ditonton";
                statusBadge.className = "text-xs font-bold px-2.5 py-1 rounded-full bg-green-100 text-green-700";
                toggleBtn.innerText = "Tandai Belum Ditonton";
            } else {
                statusBadge.innerText = "Belum ditonton";
                statusBadge.className = "text-xs font-bold px-2.5 py-1 rounded-full bg-gray-100 text-gray-600";
                toggleBtn.innerText = "Tandai Selesai ✓";
            }

            // Load saved note for this video
            const noteInput = document.getElementById('video-note-input');
            noteInput.value = videoNotes[v.id] || '';

            renderVideoPlaylist();
            renderRequiredChecklist();
        }

        function renderRequiredChecklist() {
            const container = document.getElementById('required-videos-checklist');
            if (!container) return;
            container.innerHTML = '';

            videos.forEach(v => {
                const isWatched = watchedVideos[v.id] || false;
                const card = document.createElement('div');
                card.className = "bg-white p-3.5 rounded-2xl border border-amber-200 flex items-center justify-between text-xs shadow-sm";
                card.innerHTML = `
                    <div class="space-y-0.5 pr-2">
                        <span class="text-[10px] font-bold text-nationalRed uppercase tracking-wider">${v.category}</span>
                        <div class="font-black text-nationalDark line-clamp-1">${v.title}</div>
                        <div class="text-[10px] text-gray-500 font-mono">📁 ${v.file}</div>
                    </div>
                    <button onclick="renderVideoPlayer(${v.id})" class="bg-nationalCream hover:bg-amber-100 text-nationalDark font-bold px-3 py-1.5 rounded-xl border border-amber-200 transition shrink-0">
                        Pilih ➔
                    </button>
                `;
                container.appendChild(card);
            });
        }

        function renderVideoPlaylist(filter = "Semua", query = "") {
            const grid = document.getElementById('video-playlist-grid');
            grid.innerHTML = '';

            const filtered = videos.filter(v => {
                const matchCat = (filter === "Semua" || v.category === filter);
                const matchQuery = v.title.toLowerCase().includes(query.toLowerCase()) || v.description.toLowerCase().includes(query.toLowerCase()) || v.category.toLowerCase().includes(query.toLowerCase());
                return matchCat && matchQuery;
            });

            if (filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-12 text-gray-400 font-semibold">Video tidak ditemukan.</div>`;
                return;
            }

            filtered.forEach(v => {
                const isSelected = v.id === currentVideoId;
                const isWatched = watchedVideos[v.id] || false;
                const card = document.createElement('div');
                card.className = `bg-white rounded-3xl border ${isSelected ? 'border-nationalRed ring-2 ring-red-100' : 'border-gray-200'} card-shadow overflow-hidden flex flex-col justify-between transition hover:border-nationalRed cursor-pointer`;
                card.onclick = () => renderVideoPlayer(v.id);

                card.innerHTML = `
                    <div class="relative w-full aspect-video bg-gradient-to-br from-gray-900 to-nationalDark flex flex-col items-center justify-center p-4 text-white text-center">
                        <div class="absolute top-3 left-3 bg-black/40 backdrop-blur-md px-2.5 py-0.5 rounded-full text-[10px] font-bold uppercase tracking-wider">${v.category}</div>
                        <div class="absolute top-3 right-3 bg-black/40 backdrop-blur-md px-2.5 py-0.5 rounded-full text-[10px] font-bold">⏱️ ${v.duration}</div>
                        
                        <!-- Thumbnail Mockup -->
                        <div class="text-3xl mb-1">▶</div>
                        <h4 class="text-sm font-black line-clamp-2 mt-1 px-2">${v.title}</h4>
                    </div>

                    <div class="p-4 space-y-3 flex-1 flex flex-col justify-between">
                        <div>
                            <div class="flex items-center justify-between mb-1">
                                <span class="text-[10px] font-bold text-gray-400 uppercase">${v.category}</span>
                                <span class="text-[10px] font-bold ${isWatched ? 'text-green-600 bg-green-50 px-2 py-0.5 rounded-full' : 'text-gray-400'}">${isWatched ? '✓ Selesai' : 'Belum'}</span>
                            </div>
                            <h5 class="font-bold text-nationalDark text-xs line-clamp-2">${v.title}</h5>
                        </div>
                        <button class="w-full bg-nationalCream hover:bg-red-50 hover:text-nationalRed text-nationalDark font-bold py-2 rounded-xl text-xs border border-gray-200 transition">
                            ${isSelected ? 'Sedang Diputar' : 'Tonton Video →'}
                        </button>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function setVideoFilter(cat) {
            currentVideoFilter = cat;
            document.querySelectorAll('.v-filter-btn').forEach(btn => {
                btn.className = "v-filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition";
                if (btn.innerText.toLowerCase() === cat.toLowerCase() || (cat === "Semua" && btn.innerText === "Semua")) {
                    btn.className = "v-filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-nationalRed text-white transition";
                }
            });
            const q = document.getElementById('search-video').value;
            renderVideoPlaylist(currentVideoFilter, q);
        }

        function filterVideos() {
            const q = document.getElementById('search-video').value;
            renderVideoPlaylist(currentVideoFilter, q);
        }

        function toggleWatchStatus() {
            watchedVideos[currentVideoId] = !watchedVideos[currentVideoId];
            localStorage.setItem('eshist_watched_videos', JSON.stringify(watchedVideos));
            renderVideoPlayer(currentVideoId);
            updateProgressUI();
        }

        function onVideoEnded() {
            watchedVideos[currentVideoId] = true;
            localStorage.setItem('eshist_watched_videos', JSON.stringify(watchedVideos));
            renderVideoPlayer(currentVideoId);
            updateProgressUI();
        }

        function saveVideoNote() {
            const val = document.getElementById('video-note-input').value;
            videoNotes[currentVideoId] = val;
            localStorage.setItem('eshist_video_notes', JSON.stringify(videoNotes));
            
            const savedNotice = document.getElementById('note-saved-status');
            savedNotice.classList.remove('hidden');
            setTimeout(() => {
                savedNotice.classList.add('hidden');
            }, 2000);
        }

        function loadLocalVideoFile(input) {
            if (input.files && input.files[0]) {
                const fileURL = URL.createObjectURL(input.files[0]);
                const player = document.getElementById('main-video-player');
                const source = document.getElementById('main-video-source');
                const fallback = document.getElementById('video-offline-fallback');
                
                source.src = fileURL;
                player.load();
                fallback.classList.add('hidden');
                player.classList.remove('hidden');
                player.play();
            }
        }

        function jumpToRelatedMaterial() {
            showPage('materials');
        }

        // --- TIMELINE CONTROLLER ---

        function renderTimeline() {
            const t = timelineData[currentTimelineIdx];
            
            // Render Dots
            const dotsContainer = document.getElementById('timeline-dots');
            dotsContainer.innerHTML = '';
            timelineData.forEach((item, idx) => {
                const dot = document.createElement('button');
                dot.className = `px-3 py-1.5 rounded-xl text-xs font-bold transition ${idx === currentTimelineIdx ? 'bg-nationalRed text-white shadow' : 'bg-nationalCream text-gray-600 border border-gray-200 hover:bg-gray-200'}`;
                dot.innerText = item.year;
                dot.onclick = () => {
                    currentTimelineIdx = idx;
                    renderTimeline();
                };
                dotsContainer.appendChild(dot);
            });

            // Render Card Data
            document.getElementById('tl-year').innerText = t.year;
            document.getElementById('tl-category').innerText = t.category;
            document.getElementById('tl-title').innerText = t.title;
            document.getElementById('tl-info').innerText = t.info;
            document.getElementById('tl-tokoh').innerText = t.tokoh;
            document.getElementById('tl-counter').innerText = `${currentTimelineIdx + 1} / ${timelineData.length}`;
        }

        function nextTimeline() {
            currentTimelineIdx = (currentTimelineIdx + 1) % timelineData.length;
            renderTimeline();
        }

        function prevTimeline() {
            currentTimelineIdx = (currentTimelineIdx - 1 + timelineData.length) % timelineData.length;
            renderTimeline();
        }

        function openMaterialFromTimeline() {
            const t = timelineData[currentTimelineIdx];
            showPage('materials');
            openOrgModal(t.orgId);
        }

        // --- MATERIALS & MODAL CONTROLLER ---

        function renderOrganizations(filter = "Semua", query = "") {
            const grid = document.getElementById('org-grid');
            grid.innerHTML = '';

            const filtered = organizationsData.filter(org => {
                const matchCategory = (filter === "Semua" || org.category === filter);
                const matchQuery = org.name.toLowerCase().includes(query.toLowerCase()) || org.founder.toLowerCase().includes(query.toLowerCase());
                return matchCategory && matchQuery;
            });

            if (filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-12 text-gray-400 font-semibold">Organisasi tidak ditemukan.</div>`;
                return;
            }

            filtered.forEach(org => {
                const card = document.createElement('div');
                card.className = "bg-white p-6 rounded-3xl border border-gray-200 card-shadow hover:border-nationalRed transition flex flex-col justify-between space-y-4";
                card.innerHTML = `
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="bg-red-100 text-nationalRed text-xs font-bold px-3 py-1 rounded-full">${org.year}</span>
                            <span class="text-xs font-bold text-gray-400 uppercase tracking-widest">${org.category}</span>
                        </div>
                        <h3 class="text-xl font-black text-nationalDark mb-2">${org.name}</h3>
                        <p class="text-gray-600 text-xs leading-relaxed mb-3"><strong>Fokus:</strong> ${org.focus}</p>
                    </div>
                    <div class="border-t border-gray-100 pt-3 space-y-3">
                        <div class="text-xs">
                            <span class="text-gray-400 font-bold block uppercase text-[10px]">Tokoh Utama:</span>
                            <span class="font-bold text-nationalDark">${org.founder}</span>
                        </div>
                        <button onclick="openOrgModal('${org.id}')" class="w-full bg-nationalRed hover:bg-nationalDarkRed text-white font-bold py-2.5 px-4 rounded-xl text-xs shadow transition flex items-center justify-center space-x-2">
                            <span>Buka Materi Detail</span>
                            <span>→</span>
                        </button>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function setFilter(cat) {
            activeFilter = cat;
            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.className = "filter-btn px-3.5 py-1.5 rounded-xl text-xs font-bold bg-gray-100 hover:bg-gray-200 text-gray-600 transition";
                if (btn.innerText.toLowerCase() === cat.toLowerCase() || (cat === "Semua" && btn.innerText === "Semua")) {
                    btn.className = "filter-btn active-filter px-3.5 py-1.5 rounded-xl text-xs font-bold bg-nationalRed text-white transition";
                }
            });
            const q = document.getElementById('search-org').value;
            renderOrganizations(activeFilter, q);
        }

        function filterOrganizations() {
            const q = document.getElementById('search-org').value;
            renderOrganizations(activeFilter, q);
        }

        function openOrgModal(orgId) {
            const org = organizationsData.find(o => o.id === orgId);
            if (!org) return;

            const modalContent = document.getElementById('modal-content');
            modalContent.innerHTML = `
                <div class="space-y-4">
                    <div class="flex items-center space-x-3">
                        <span class="bg-red-100 text-nationalRed text-xs font-bold px-3 py-1 rounded-full">${org.year}</span>
                        <h3 class="text-2xl font-black text-nationalDark">${org.name}</h3>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-3 bg-nationalCream p-4 rounded-2xl border border-gray-200 text-xs">
                        <div>
                            <span class="font-bold text-gray-400 uppercase">Tokoh / Pendiri:</span>
                            <p class="font-bold text-nationalDark mt-0.5">${org.founder}</p>
                        </div>
                        <div>
                            <span class="font-bold text-gray-400 uppercase">Fokus Perjuangan:</span>
                            <p class="font-bold text-nationalDark mt-0.5">${org.focus}</p>
                        </div>
                    </div>

                    <div class="space-y-3 text-xs text-gray-700 leading-relaxed">
                        <div class="bg-gray-50 p-3.5 rounded-xl">
                            <h4 class="font-bold text-nationalDark mb-1">🏛️ Latar Belakang:</h4>
                            <p>${org.background}</p>
                        </div>
                        <div class="bg-gray-50 p-3.5 rounded-xl">
                            <h4 class="font-bold text-nationalDark mb-1">🎯 Tujuan Organisasi:</h4>
                            <p>${org.purpose}</p>
                        </div>
                        <div class="bg-gray-50 p-3.5 rounded-xl">
                            <h4 class="font-bold text-nationalDark mb-1">⭐ Peran dalam Pergerakan Nasional:</h4>
                            <p>${org.role}</p>
                        </div>
                        <div class="bg-red-50 text-red-900 p-3.5 rounded-xl font-medium">
                            <h4 class="font-bold mb-1">💡 Fakta Menarik:</h4>
                            <p>${org.fact}</p>
                        </div>
                    </div>
                </div>
            `;
            document.getElementById('org-modal').classList.remove('hidden');
            document.getElementById('org-modal').classList.add('flex');
            markPageVisited('materials');
        }

        function closeOrgModal() {
            document.getElementById('org-modal').classList.add('hidden');
            document.getElementById('org-modal').classList.remove('flex');
        }

        // --- MEMORY MATCH GAME CONTROLLER ---

        function startMemoryGame() {
            clearInterval(gameInterval);
            gameTimer = 0;
            gameTries = 0;
            matchedPairs = 0;
            flippedCards = [];
            isGameLocked = false;

            document.getElementById('game-score').innerText = '0';
            document.getElementById('game-tries').innerText = '0';
            document.getElementById('game-timer').innerText = '00:00';

            gameInterval = setInterval(() => {
                gameTimer++;
                const m = Math.floor(gameTimer / 60).toString().padStart(2, '0');
                const s = (gameTimer % 60).toString().padStart(2, '0');
                document.getElementById('game-timer').innerText = `${m}:${s}`;
            }, 1000);

            let deck = [];
            memoryPairs.forEach((pair, idx) => {
                deck.push({ id: idx, type: 'org', text: pair.org, pairId: idx });
                deck.push({ id: idx + 100, type: 'match', text: pair.match, pairId: idx });
            });

            deck.sort(() => Math.random() - 0.5);
            gameCards = deck;

            const board = document.getElementById('memory-board');
            board.innerHTML = '';
            board.className = "grid grid-cols-3 sm:grid-cols-4 gap-3 sm:gap-4 items-center justify-center";

            gameCards.forEach((card, index) => {
                const btn = document.createElement('button');
                btn.className = "h-24 sm:h-28 rounded-2xl bg-nationalCream border-2 border-gray-300 font-bold text-xs sm:text-sm text-gray-400 flex items-center justify-center p-3 text-center transition transform hover:scale-105 shadow-sm";
                btn.dataset.index = index;
                btn.innerHTML = `<span>❓</span>`;
                btn.onclick = () => flipCard(index, btn);
                board.appendChild(btn);
            });
            markPageVisited('game');
        }

        function flipCard(index, btn) {
            if (isGameLocked) return;
            const card = gameCards[index];
            if (btn.classList.contains('matched') || btn.classList.contains('flipped')) return;

            btn.classList.add('flipped', 'bg-white', 'border-nationalRed', 'text-nationalDark');
            btn.innerHTML = `<span class="font-black">${card.text}</span>`;
            flippedCards.push({ index, card, btn });

            if (flippedCards.length === 2) {
                gameTries++;
                document.getElementById('game-tries').innerText = gameTries;
                checkMatch();
            }
        }

        function checkMatch() {
            isGameLocked = true;
            const [c1, c2] = flippedCards;

            if (c1.card.pairId === c2.card.pairId && c1.index !== c2.index) {
                c1.btn.classList.add('bg-green-100', 'border-green-500', 'matched');
                c2.btn.classList.add('bg-green-100', 'border-green-500', 'matched');
                matchedPairs++;
                document.getElementById('game-score').innerText = matchedPairs;
                flippedCards = [];
                isGameLocked = false;

                if (matchedPairs === memoryPairs.length) {
                    clearInterval(gameInterval);
                    document.getElementById('game-win-stats').innerText = `Selesai dalam ${gameTries} percobaan dan waktu ${document.getElementById('game-timer').innerText}.`;
                    document.getElementById('game-win-modal').classList.remove('hidden');
                    document.getElementById('game-win-modal').classList.add('flex');
                    markPageVisited('game');
                }
            } else {
                setTimeout(() => {
                    c1.btn.classList.remove('flipped', 'bg-white', 'border-nationalRed', 'text-nationalDark');
                    c1.btn.className = "h-24 sm:h-28 rounded-2xl bg-nationalCream border-2 border-gray-300 font-bold text-xs sm:text-sm text-gray-400 flex items-center justify-center p-3 text-center transition transform hover:scale-105 shadow-sm";
                    c1.btn.innerHTML = `<span>❓</span>`;

                    c2.btn.classList.remove('flipped', 'bg-white', 'border-nationalRed', 'text-nationalDark');
                    c2.btn.className = "h-24 sm:h-28 rounded-2xl bg-nationalCream border-2 border-gray-300 font-bold text-xs sm:text-sm text-gray-400 flex items-center justify-center p-3 text-center transition transform hover:scale-105 shadow-sm";
                    c2.btn.innerHTML = `<span>❓</span>`;

                    flippedCards = [];
                    isGameLocked = false;
                }, 1000);
            }
        }

        function closeGameModal() {
            document.getElementById('game-win-modal').classList.add('hidden');
            document.getElementById('game-win-modal').classList.remove('flex');
        }

        // --- EVALUATION QUIZ CONTROLLER ---

        function renderQuizQuestion() {
            const q = questionsData[currentQuizIdx];
            document.getElementById('quiz-progress-label').innerText = `Soal ${currentQuizIdx + 1} dari ${questionsData.length}`;
            document.getElementById('quiz-question-text').innerText = `${currentQuizIdx + 1}. ${q.q}`;
            document.getElementById('quiz-feedback').innerHTML = '';
            document.getElementById('quiz-submit-btn').innerText = "Jawab & Lanjut →";

            const optionsList = document.getElementById('quiz-options-list');
            optionsList.innerHTML = '';

            q.options.forEach((opt, idx) => {
                const label = document.createElement('label');
                label.className = "flex items-center space-x-3 p-3.5 rounded-xl border border-gray-200 bg-gray-50/50 hover:bg-gray-50 cursor-pointer transition text-xs sm:text-sm font-semibold text-gray-700";
                
                const checked = userAnswers[currentQuizIdx] === idx ? 'checked' : '';
                label.innerHTML = `
                    <input type="radio" name="quiz_opt" value="${idx}" ${checked} class="text-nationalRed focus:ring-nationalRed w-4 h-4">
                    <span>${opt}</span>
                `;
                optionsList.appendChild(label);
            });
        }

        function submitQuizAnswer() {
            const selected = document.querySelector('input[name="quiz_opt"]:checked');
            if (!selected) {
                // Custom non-alert warning notice inside feedback box instead of alert()
                document.getElementById('quiz-feedback').innerHTML = `<span class="text-amber-600 font-bold">Harap pilih salah satu jawaban terlebih dahulu!</span>`;
                return;
            }

            const chosenIdx = parseInt(selected.value);
            userAnswers[currentQuizIdx] = chosenIdx;

            const q = questionsData[currentQuizIdx];
            const feedbackBox = document.getElementById('quiz-feedback');
            
            if (chosenIdx === q.answer) {
                feedbackBox.innerHTML = `<span class="text-green-600 font-bold">✓ Benar!</span>`;
            } else {
                feedbackBox.innerHTML = `<span class="text-red-600 font-bold">✕ Kurang tepat.</span>`;
            }

            setTimeout(() => {
                currentQuizIdx++;
                if (currentQuizIdx < questionsData.length) {
                    renderQuizQuestion();
                } else {
                    renderQuizResults();
                }
            }, 800);
        }

        function renderQuizResults() {
            let correct = 0;
            questionsData.forEach((q, idx) => {
                if (userAnswers[idx] === q.answer) correct++;
            });

            const wrong = questionsData.length - correct;
            const score = correct * 5;
            const pct = Math.round((correct / questionsData.length) * 100);

            let msg = "";
            if (score >= 90) msg = "Pemahamanmu terhadap materi Pergerakan Nasional sudah sangat baik.";
            else if (score >= 75) msg = "Pemahamanmu sudah baik. Beberapa materi masih perlu diperkuat.";
            else if (score >= 60) msg = "Pemahamanmu cukup. Coba pelajari kembali materi organisasi dan tokoh.";
            else msg = "Nilai di bawah KKM. Silakan pelajari kembali modul materi organisasi dan video edukasi.";

            document.getElementById('quiz-box').classList.add('hidden');
            const resBox = document.getElementById('quiz-results-box');
            resBox.classList.remove('hidden');
            resBox.innerHTML = `
                <div class="w-16 h-16 bg-nationalRed text-white rounded-full flex items-center justify-center mx-auto text-2xl shadow-md">
                    🎓
                </div>
                <div>
                    <h3 class="text-2xl font-black text-nationalDark">Evaluasi Selesai</h3>
                    <div class="text-4xl font-black text-nationalRed mt-2">${score} <span class="text-sm text-gray-400 font-normal">/ 100</span></div>
                </div>
                <div class="grid grid-cols-3 gap-3 max-w-sm mx-auto bg-nationalCream p-3 rounded-2xl border border-gray-200 text-xs font-bold text-gray-700">
                    <div>Benar: <span class="text-green-600">${correct}</span></div>
                    <div>Salah: <span class="text-red-600">${wrong}</span></div>
                    <div>Akurasi: <span class="text-nationalDark">${pct}%</span></div>
                </div>
                <p class="text-xs sm:text-sm font-semibold text-gray-600 italic max-w-md mx-auto">${msg}</p>
                <div class="flex flex-col sm:flex-row justify-center gap-3 pt-4">
                    <button onclick="resetQuiz()" class="bg-nationalCream hover:bg-gray-200 text-nationalDark font-bold py-3 px-6 rounded-xl text-xs border border-gray-200 transition">
                        Ulangi Evaluasi
                    </button>
                    <button onclick="showPage('home')" class="bg-nationalRed hover:bg-nationalDarkRed text-white font-bold py-3 px-6 rounded-xl text-xs shadow transition">
                        Kembali ke Beranda
                    </button>
                    <button onclick="showPage('videos')" class="bg-nationalDark hover:bg-black text-white font-bold py-3 px-6 rounded-xl text-xs shadow transition">
                        Tonton Video Edukasi
                    </button>
                </div>
            `;
            markPageVisited('evaluation');
        }

        function resetQuiz() {
            currentQuizIdx = 0;
            userAnswers = {};
            document.getElementById('quiz-results-box').classList.add('hidden');
            document.getElementById('quiz-box').classList.remove('hidden');
            renderQuizQuestion();
        }

        // --- INITIALIZATION ---

        window.onload = function() {
            loadProgress();
            renderTimeline();
            renderVideoPlayer(currentVideoId);
            renderVideoPlaylist();
            renderRequiredChecklist();
            renderOrganizations();
            renderQuizQuestion();
        };
    </script>
</body>
</html>
