EXE = roadfighter
SRC = src
PREFIX = /usr/local
EXTRADIR = ./build/linux

GAMEDIR = $(PREFIX)/games
STARTUP = $(GAMEDIR)/$(EXE)
BINDIR = $(PREFIX)/share/games/$(EXE)
ICNDIR = $(PREFIX)/share/pixmaps
APPDIR = $(PREFIX)/share/applications

DATA = fonts graphics maps sound readme.txt
ICON = roadfighter.png
DESKTOP = roadfighter.desktop

OBJS = \
	$(SRC)/sge/sge_blib.o			$(SRC)/sge/sge_bm_text.o			\
	$(SRC)/sge/sge_collision.o		$(SRC)/sge/sge_misc.o				\
	$(SRC)/sge/sge_primitives.o		$(SRC)/sge/sge_rotation.o			\
	$(SRC)/sge/sge_shape.o			$(SRC)/sge/sge_surface.o			\
	$(SRC)/sge/sge_textpp.o			$(SRC)/sge/sge_tt_text.o			\
	$(SRC)/CCarObject.o				$(SRC)/CEnemyCarObject.o			\
	$(SRC)/CEnemyFastCarObject.o	$(SRC)/CEnemyNormalCarObject.o		\
	$(SRC)/CEnemyRacerCarObject.o	$(SRC)/CEnemySlidderCarObject.o		\
	$(SRC)/CEnemyTruckObject.o		$(SRC)/CExplosionObject.o			\
	$(SRC)/CFuelObject.o			$(SRC)/CGame.o						\
	$(SRC)/CObject.o				$(SRC)/CPlayerCarObject.o			\
	$(SRC)/CRoadFighter.o			$(SRC)/CSemaphoreObject.o			\
	$(SRC)/CTile.o														\
	$(SRC)/auxiliar.o				$(SRC)/configuration.o				\
	$(SRC)/debug.o					$(SRC)/filehandling.o				\
	$(SRC)/gameover_state.o			$(SRC)/interlevel_state.o			\
	$(SRC)/konami_state.o			$(SRC)/loadmg2.o					\
	$(SRC)/menu_state.o				$(SRC)/playing_state.o				\
	$(SRC)/presentation_state.o		$(SRC)/quick_tables.o				\
	$(SRC)/sound.o														\
	$(SRC)/main.o

CC = g++
ifeq ($(ODROID),1)
 CFLAGS = -DODROID -mcpu=cortex-a9 -mfpu=neon -mfloat-abi=hard -ftree-vectorize -fsingle-precision-constant -g -Ofast `sdl-config --cflags` -I/usr/X11R6/include
else ifeq ($(LINUX),1)
 CFLAGS = -mcpu=native -Ofast -I/usr/include/SDL -I/usr/X11R6/include
else
 CFLAGS = -DPANDORA -mcpu=cortex-a8 -mfpu=neon -mfloat-abi=softfp -ftree-vectorize -fsingle-precision-constant -g -Ofast `sdl-config --cflags` -I/usr/X11R6/include
endif
LDFLAGS = `sdl-config --libs` -L/usr/X11R6/lib/ -lSDL_image -lSDL_mixer -lSDL_sound -lSDL_ttf
RM = rm -f
CP = cp -r
MD = mkdir -p
ECHO = echo
CHMOD = chmod
STRIP = strip

all: $(EXE)

%.o: %.cpp
	$(CC) $(CFLAGS) -c $< -o $@

$(EXE): $(OBJS)
	$(CC) $^ -o $@ $(LDFLAGS)
	@$(STRIP) $@
	@$(ECHO) " o If there are no errors, the game compiled succesfully"

clean:
	@$(ECHO) " o Cleaning up source directory"
	@$(RM) $(SRC)/*.o $(SRC)/sge/*.o $(SRC)/*.bak core $(EXE)

install: all
	@$(ECHO) " o Creating install directory $(BINDIR)"
	@$(MD) "$(BINDIR)"
	@$(ECHO) " o Installing game and data to $(BINDIR)"
	@$(CP) "$(EXE)" $(DATA) "$(BINDIR)"
	@$(ECHO) " o Creating startup script $(STARTUP)"
	@$(MD) "$(GAMEDIR)"
	@$(ECHO) "cd \"$(BINDIR)\"; ./$(EXE); cd -" >"$(STARTUP)"
	@$(CHMOD) 755 "$(STARTUP)"
	@$(ECHO) " o Creating application menu entry"
	@$(MD) "$(ICNDIR)"
	@$(CP) "$(EXTRADIR)/$(ICON)" "$(ICNDIR)"
	@$(MD) "$(APPDIR)"
	@$(CP) "$(EXTRADIR)/$(DESKTOP)" "$(APPDIR)"
	@$(ECHO) ""

uninstall:
	@$(ECHO) " o Removing game and data from $(BINDIR)"
	@$(RM) -r "$(BINDIR)"
	@$(ECHO) " o Removing startup script $(STARTUP)"
	@$(RM) "$(STARTUP)"
	@$(ECHO) " o Removing application menu entry"
	@$(RM) "$(ICNDIR)/$(ICON)" "$(APPDIR)/$(DESKTOP)"
	@$(ECHO) ""

