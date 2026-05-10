# MAKEFILE UPDATE - Добавьте эти строки в соответствующие места вашего Makefile

# === НОВОЕ: После строки 345 (где копируются dylib) ===

# В целевой функции 'payload' добавьте после строки:
# cp $(WORKINGDIR)/*.dylib $(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks/ || exit 1

sable_frameworks:
	echo '[Amethyst v$(VERSION)] sable - frameworks - start'
	mkdir -p $(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks
	# Копируем все dylib из Natives/resources/Frameworks (включая Sable)
	if [ -d "Natives/resources/Frameworks" ]; then \
		find Natives/resources/Frameworks -maxdepth 1 -name "*.dylib" -exec cp {} $(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks/ \; 2>/dev/null || true; \
		echo "✅ Sable dylib copied to app bundle"; \
	fi
	# Подписываем все dylib в Frameworks
	if [ -d "$(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks" ]; then \
		find $(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks -name "*.dylib" -exec ldid -S {} \; 2>/dev/null || true; \
		echo "✅ All dylibs signed"; \
	fi
	echo '[Amethyst v$(VERSION)] sable - frameworks - end'

# === ОБНОВЛЕНИЕ целевой функции payload ===
# Измените целевую функцию 'payload' со строки 338, добавив зависимость sable_frameworks:

# ДО:
# payload: native dep_mg java jre assets

# ПОСЛЕ:
# payload: native dep_mg java jre assets sable_frameworks

# Затем убедитесь, что копирование всех dylib выглядит так:
# cp $(WORKINGDIR)/*.dylib $(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks/ || exit 1
# find Natives/resources/Frameworks -maxdepth 1 -name "*.dylib" -exec cp {} $(WORKINGDIR)/AngelAuraAmethyst.app/Frameworks/ \; 2>/dev/null || true
