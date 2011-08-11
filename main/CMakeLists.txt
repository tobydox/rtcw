SET(ui_SOURCES
  ${S}/ui/ui_main.c
  ${S}/ui/ui_atoms.c
  ${S}/ui/ui_gameinfo.c
  ${S}/ui/ui_players.c
  ${S}/ui/ui_shared.c
  ${S}/ui/ui_syscalls.c

  ${S}/game/bg_misc.c
  ${S}/game/q_math.c
  ${S}/game/q_shared.c
	)

SET(cgame_SOURCES
  ${S}/cgame/cg_main.c
  ${S}/game/bg_animation.c
  ${S}/game/bg_misc.c
  ${S}/game/bg_pmove.c
  ${S}/game/bg_slidemove.c
  ${S}/cgame/cg_consolecmds.c
  ${S}/cgame/cg_draw.c
  ${S}/cgame/cg_drawtools.c
  ${S}/cgame/cg_effects.c
  ${S}/cgame/cg_ents.c
  ${S}/cgame/cg_event.c
  ${S}/cgame/cg_flamethrower.c
  ${S}/cgame/cg_info.c
  ${S}/cgame/cg_localents.c
  ${S}/cgame/cg_marks.c
  ${S}/cgame/cg_newDraw.c
  ${S}/cgame/cg_particles.c
  ${S}/cgame/cg_players.c
  ${S}/cgame/cg_playerstate.c
  ${S}/cgame/cg_predict.c
  ${S}/cgame/cg_scoreboard.c
  ${S}/cgame/cg_servercmds.c
  ${S}/cgame/cg_sound.c
  ${S}/cgame/cg_snapshot.c
  ${S}/cgame/cg_trails.c
  ${S}/cgame/cg_view.c
  ${S}/cgame/cg_weapons.c

  ${S}/ui/ui_shared.c

  ${S}/game/q_math.c
  ${S}/game/q_shared.c

  ${S}/cgame/cg_syscalls.c
)

SET(qagame_SOURCES
  ${S}/game/g_main.c
  ${S}/game/ai_cast.c
  ${S}/game/ai_cast_characters.c
  ${S}/game/ai_cast_debug.c
  ${S}/game/ai_cast_events.c
  ${S}/game/ai_cast_fight.c
  ${S}/game/ai_cast_func_attack.c
  ${S}/game/ai_cast_func_boss1.c
  ${S}/game/ai_cast_funcs.c
  ${S}/game/ai_cast_script.c
  ${S}/game/ai_cast_script_actions.c
  ${S}/game/ai_cast_script_ents.c
  ${S}/game/ai_cast_sight.c
  ${S}/game/ai_cast_think.c
  ${S}/botai/ai_chat.c
  ${S}/botai/ai_cmd.c
  ${S}/botai/ai_dmnet.c
  ${S}/botai/ai_dmq3.c
  ${S}/botai/ai_main.c
  ${S}/botai/ai_team.c
  ${S}/game/bg_misc.c
  ${S}/game/bg_pmove.c
  ${S}/game/bg_slidemove.c
  ${S}/game/bg_animation.c
  ${S}/game/g_active.c
  ${S}/game/g_alarm.c
  ${S}/game/g_bot.c
  ${S}/game/g_client.c
  ${S}/game/g_cmds.c
  ${S}/game/g_combat.c
  ${S}/game/g_items.c
  ${S}/game/g_mem.c
  ${S}/game/g_misc.c
  ${S}/game/g_missile.c
  ${S}/game/g_mover.c
  ${S}/game/g_props.c
  ${S}/game/g_save.c
  ${S}/game/g_script_actions.c
  ${S}/game/g_script.c
  ${S}/game/g_session.c
  ${S}/game/g_spawn.c
  ${S}/game/g_svcmds.c
  ${S}/game/g_target.c
  ${S}/game/g_team.c
  ${S}/game/g_tramcar.c
  ${S}/game/g_trigger.c
  ${S}/game/g_utils.c
  ${S}/game/g_weapon.c

  ${S}/game/q_math.c
  ${S}/game/q_shared.c
  ${S}/game/g_syscalls.c
)

IF(IS_X86)
	SET(UI_LIB uii386)
	SET(QAGAME_LIB qagamei386)
	SET(CGAME_LIB cgamei386)
ENDIF()
IF(IS_X86_64)
	SET(UI_LIB uix86_64)
	SET(QAGAME_LIB qagamex86_64)
	SET(CGAME_LIB cgamex86_64)
ENDIF()

ADD_LIBRARY(${UI_LIB} SHARED ${ui_SOURCES})
ADD_LIBRARY(${QAGAME_LIB} SHARED ${qagame_SOURCES})
ADD_LIBRARY(${CGAME_LIB} SHARED ${cgame_SOURCES})
SET_TARGET_PROPERTIES(${UI_LIB} PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(${CGAME_LIB} PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(${QAGAME_LIB} PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(${CGAME_LIB} PROPERTIES COMPILE_FLAGS "-DCGAME -DCGAMEDLL")
SET_TARGET_PROPERTIES(${QAGAME_LIB} PROPERTIES COMPILE_FLAGS "-DQAGAME -DGAMEDLL")

