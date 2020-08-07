/* See LICENSE file for copyright and license details. */

/* appearance */
static unsigned int borderpx  = 3;        /* border pixel of windows */
static unsigned int gappx     = 8;        /* gaps between windows */
static unsigned int snap      = 32;       /* snap pixel */
static int showbar            = 1;        /* 0 means no bar */
static int topbar             = 1;        /* 0 means bottom bar */
static int horizpadbar        = 2;        /* INNER horizontal padding for statusbar */
static int vertpadbar         = 8;        /* INNER vertical padding for statusbar */
static int vertpad            = 5;       /* OUTER vertical padding of bar */
static int sidepad            = 10;       /* OUTER horizontal padding of bar */
static char *fonts[]          = { "Inconsolata Nerd Font:size=12" };
static char dmenufont[]       = "Inconsolata Nerd Font:size=12";

static char normbgcolor[]           = "#222222";
static char normbordercolor[]       = "#444444";
static char normfgcolor[]           = "#bbbbbb";
static char selfgcolor[]            = "#eeeeee";
static char selbordercolor[]        = "#005577";
static char selbgcolor[]            = "#005577";
static char *colors[][3] = {
       /*               fg           bg           border   */
       [SchemeNorm] = { normfgcolor, normbgcolor, normbordercolor },
       [SchemeSel]  = { selfgcolor,  selbgcolor,  selbordercolor  },
 };
;
static const unsigned int alphas[][3]      = {
	/*               fg      bg        border     */
	[SchemeNorm] = { OPAQUE, baralpha, borderalpha },
	[SchemeSel]  = { OPAQUE, baralpha, borderalpha },
};

/* tagging */
static const char *tags[] = { "ﲵ ", " ", " ", " ", "ﭮ ", " ", " ", " ", " " };
static const char *tagsalt[] = { "1", "2", "3", "4", "5", "6", "7", "8", "9" };

static const Rule rules[] = {
	/* xprop(1):
	 *	WM_CLASS(STRING) = instance, class
	 *	WM_NAME(STRING) = title
	 */
	/* class      instance    title       tags mask     isfloating   monitor */
	{ "Firefox",  NULL,       NULL,       1 << 8,       0,           -1 },
	{ "Telegram", NULL,       NULL,       1 << 3,       0,           -1 },
	{ "discord",  NULL,       NULL,       1 << 4,       0,           -1 },
};

/* layout(s) */
static const float mfact     = 0.5; /* factor of master area size [0.05..0.95] */
static const int nmaster     = 1;    /* number of clients in master area */
static const int resizehints = 1;    /* 1 means respect size hints in tiled resizals */

static const Layout layouts[] = {
	/* symbol     arrange function */
	{ "[]=",      tile },    /* first entry is default */
	{ "><>",      NULL },    /* no layout function means floating behavior */
	{ "[M]",      monocle },
};

/* key definitions */
#define MODKEY Mod4Mask
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },

/* helper for spawning shell commands in the pre dwm-5.0 fashion */
#define SHCMD(cmd) { .v = (const char*[]){ "/bin/sh", "-c", cmd, NULL } }

/* commands */
static char dmenumon[2] = "0"; /* component of dmenucmd, manipulated in spawn() */
static const char *dmenucmd[] = { "dmenu_run", "-b", "-m", dmenumon, "-fn", dmenufont, "-nb", normbgcolor, "-nf", normfgcolor, "-sb", selbordercolor, "-sf", selfgcolor, NULL };
 
static const char *termcmd[]  = { "st", NULL };
static const char *incBackLight[] = {"xbacklight", "+5", NULL};
static const char *decBackLight[] = {"xbacklight", "-5", NULL};
static const char *incAlsa[] = {"amixer", "-c", "1", "set", "Master", "2%+", NULL};
static const char *decAlsa[] = {"amixer", "-c", "1", "set", "Master", "2%-", NULL};
static const char *muteAlsa[] = {"amixer", "-c", "1", "set", "Master", "0%", NULL};
static const char *unmuteAlsa[] = {"amixer", "-c", "1", "set", "Master", "50%", NULL};
static const char *toggleMpc[] = { "mpc", "toggle", NULL};
static const char *stopMpc[] = { "mpc", "stop", NULL};
static const char *nextMpc[] = { "mpc", "next", NULL};
static const char *prevMpc[] = { "mpc", "prev", NULL};
static const char *seekForvardMpc[] = { "mpc", "seek", "+15", NULL};
static const char *seekBackwardMpc[] = { "mpc", "seek", "-15", NULL};
static const char *ranger[]  = { "st", "-e", "ranger", NULL };
static const char *slock[]  = { "slock", NULL };
static const char *screenShootEntireX[] = { "import", "-window", "root", "/home/wvovaw/Images/Screenshots/shoot.png", NULL };
static const char *screenShootSelect[] = { "import", "/home/wvovaw/Images/Screenshots/shoot.png", NULL };

+/*
+ * Xresources preferences to load at startup
+ */
ResourcePref resources[] = {
		{ "normbgcolor",        STRING,  &normbgcolor },
		{ "normbordercolor",    STRING,  &normbordercolor },
		{ "normfgcolor",        STRING,  &normfgcolor },
		{ "selbgcolor",         STRING,  &selbgcolor },
		{ "selbordercolor",     STRING,  &selbordercolor },
		{ "selfgcolor",         STRING,  &selfgcolor },
		{ "borderpx",          	INTEGER, &borderpx },
		{ "snap",          		INTEGER, &snap },
		{ "showbar",          	INTEGER, &showbar },
		{ "topbar",          	INTEGER, &topbar },
		{ "nmaster",          	INTEGER, &nmaster },
		{ "resizehints",       	INTEGER, &resizehints },
		{ "mfact",      	 	FLOAT,   &mfact },
};

static Key keys[] = {
	/* modifier                     key        function        argument */
	{ MODKEY,                       XK_F1,     spawn,          {.v = dmenucmd } },
	{ MODKEY|ShiftMask,             XK_Return, spawn,          {.v = termcmd } },
	{ MODKEY,                       XK_b,      togglebar,      {0} },
	{ MODKEY,                       XK_j,      focusstack,     {.i = +1 } },
	{ MODKEY,                       XK_k,      focusstack,     {.i = -1 } },
	{ MODKEY,                       XK_i,      incnmaster,     {.i = +1 } },
	{ MODKEY,                       XK_d,      incnmaster,     {.i = -1 } },
	{ MODKEY,                       XK_h,      setmfact,       {.f = -0.05} },
	{ MODKEY,                       XK_l,      setmfact,       {.f = +0.05} },
	{ MODKEY,                       XK_Return, zoom,           {0} },
	{ MODKEY,                       XK_Tab,    view,           {0} },
	{ MODKEY|ShiftMask,             XK_c,      killclient,     {0} },
	{ MODKEY,                       XK_t,      setlayout,      {.v = &layouts[0]} },
	{ MODKEY,                       XK_f,      setlayout,      {.v = &layouts[1]} },
	{ MODKEY,                       XK_m,      setlayout,      {.v = &layouts[2]} },
	{ MODKEY,                       XK_space,  setlayout,      {0} },
	{ MODKEY|ShiftMask,             XK_space,  togglefloating, {0} },
	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } },
	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } },
	{ MODKEY,                       XK_comma,  focusmon,       {.i = -1 } },
	{ MODKEY,                       XK_period, focusmon,       {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_comma,  tagmon,         {.i = -1 } },
	{ MODKEY|ShiftMask,             XK_period, tagmon,         {.i = +1 } },
	{ MODKEY,                       XK_minus,  setgaps,        {.i = -1 } },
	{ MODKEY,                       XK_equal,  setgaps,        {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_equal,  setgaps,        {.i = 0  } },
	{ MODKEY|ShiftMask,             XK_n,      togglealttag,   {0} },
        { MODKEY,                       XK_Left,   spawn,          {.v = decBackLight} },
        { MODKEY,                       XK_Right,  spawn,          {.v = incBackLight} },
        { MODKEY,                       XK_F2,     spawn,          {.v = decAlsa} },
        { MODKEY,                       XK_F3,     spawn,          {.v = incAlsa} },
        { MODKEY,                       XK_F4,     spawn,          {.v = muteAlsa} },
        { MODKEY|ShiftMask,             XK_F4,     spawn,          {.v = unmuteAlsa} },
        { MODKEY,                       XK_F5,     spawn,          {.v = prevMpc} },
        { MODKEY,                       XK_F6,     spawn,          {.v = nextMpc} },
        { MODKEY|ShiftMask,             XK_F5,     spawn,          {.v = seekBackwardMpc} },
        { MODKEY|ShiftMask,             XK_F6,     spawn,          {.v = seekForvardMpc} },
        { MODKEY,                       XK_F7,     spawn,          {.v = toggleMpc} },
        { MODKEY,                       XK_F8,     spawn,          {.v = stopMpc} },
        { MODKEY,                       XK_F11,    spawn,          {.v = slock} },
        { MODKEY|ShiftMask,             XK_r,      spawn,          {.v = ranger} },
        { MODKEY,                       XK_Print,  spawn,          {.v = screenShootEntireX} },
        { MODKEY|ShiftMask,             XK_Print,  spawn,          {.v = screenShootSelect} },
	TAGKEYS(                        XK_1,                      0)
	TAGKEYS(                        XK_2,                      1)
	TAGKEYS(                        XK_3,                      2)
	TAGKEYS(                        XK_4,                      3)
	TAGKEYS(                        XK_5,                      4)
	TAGKEYS(                        XK_6,                      5)
	TAGKEYS(                        XK_7,                      6)
	TAGKEYS(                        XK_8,                      7)
	TAGKEYS(                        XK_9,                      8)
	{ MODKEY|ShiftMask,             XK_q,      quit,           {0} },
};

/* button definitions */
/* click can be ClkTagBar, ClkLtSymbol, ClkStatusText, ClkWinTitle, ClkClientWin, or ClkRootWin */
static Button buttons[] = {
	/* click                event mask      button          function        argument */
	{ ClkLtSymbol,          0,              Button1,        setlayout,      {0} },
	{ ClkLtSymbol,          0,              Button3,        setlayout,      {.v = &layouts[2]} },
	{ ClkWinTitle,          0,              Button2,        zoom,           {0} },
	{ ClkStatusText,        0,              Button2,        spawn,          {.v = termcmd } },
	{ ClkClientWin,         MODKEY,         Button1,        movemouse,      {0} },
	{ ClkClientWin,         MODKEY,         Button2,        togglefloating, {0} },
	{ ClkClientWin,         MODKEY,         Button3,        resizemouse,    {0} },
	{ ClkTagBar,            0,              Button1,        view,           {0} },
	{ ClkTagBar,            0,              Button3,        toggleview,     {0} },
	{ ClkTagBar,            MODKEY,         Button1,        tag,            {0} },
	{ ClkTagBar,            MODKEY,         Button3,        toggletag,      {0} },
};

