mohamsirr
=========

 package com.rs.cache.loaders; import java.util.ArrayList; import java.util.Arrays; import java.util.HashMap; import java.util.List;  import com.alex.utils.Constants; import com.rs.cache.Cache; import com.rs.game.item.Item; import com.rs.game.player.Equipment; import com.rs.game.player.Skills; import com.rs.io.InputStream; import com.rs.utils.Utils; @SuppressWarnings("unused") public final class ItemDefinitions { 	public static final ItemDefinitions[] itemsDefinitions;  	static {  		itemsDefinitions = new ItemDefinitions[Utils.getItemDefinitionsSize()]; 	} 	public int id; 	public boolean loaded; 	public int modelId; 	public String name; 	public static ItemDefinitions forName(String name) { 		for (ItemDefinitions definition : itemsDefinitions) { 			if (definition.name.equalsIgnoreCase(name)) { 				return definition; 			} 		} 		return null; 	} 	public int modelZoom; 	public int modelRotation1; 	public int modelRotation2; 	public int modelOffset1; 	public int modelOffset2; 	public int stackable; 	public int value; 	public boolean membersOnly;  	public int maleEquip1; 	public int femaleEquip1; 	public int maleEquip2; 	public int femaleEquip2; 	public String[] groundOptions; 	public String[] inventoryOptions; 	public int[] originalModelColors; 	public int[] modifiedModelColors; 	public short[] originalTextureColors; 	public short[] modifiedTextureColors; 	public byte[] unknownArray1; 	public byte[] unknownArray3; 	public int[] unknownArray2; 	public boolean unnoted; 	public int maleEquipModelId3; 	public int femaleEquipModelId3; 	public static int unknownInt1; 	public int unknownInt2; 	public int unknownInt3; 	public int unknownInt4; 	public int unknownInt5; 	public int unknownInt6; 	public int certId; 	public int certTemplateId; 	public int[] stackIds; 	public int[] stackAmounts; 	public int unknownInt7; 	public int unknownInt8; 	public int unknownInt9; 	public int unknownInt10; 	public int unknownInt11; 	public int teamId; 	public int lendId; 	public int lendTemplateId; 	public int unknownInt12; 	public int unknownInt13; 	public int unknownInt14; 	public int unknownInt15; 	public int unknownInt16; 	public int unknownInt17; 	public int unknownInt18; 	public int unknownInt19; 	public int unknownInt20; 	public int unknownInt21; 	public int unknownInt22; 	public int unknownInt23; 	public int equipSlot; 	public int equipType;  	public boolean noted; 	public boolean lended;  	public HashMap&lt;Integer, Object> clientScriptData; 	public HashMap&lt;Integer, Integer> itemRequiriments; 	public int[] unknownArray5; 	public int[] unknownArray4; 	public byte[] unknownArray6;  	public static final ItemDefinitions getItemDefinitions(int itemId) { 		if (itemId &lt; 0 || itemId >= itemsDefinitions.length) 			itemId = 0; 		ItemDefinitions def = itemsDefinitions[itemId]; 		if (def == null) 			itemsDefinitions[itemId] = def = new ItemDefinitions(itemId); 		return def; 	}  	public static final void clearItemsDefinitions() { 		for (int i = 0; i &lt; itemsDefinitions.length; i++) 			itemsDefinitions[i] = null; 	}  	public ItemDefinitions(int id) { 		this.id = id; 		setDefaultsVariableValues(); 		setDefaultOptions(); 		loadItemDefinitions(); 	}  	public boolean isLoaded() { 		return loaded; 	}  	public final void loadItemDefinitions() { 		byte[] data = Cache.STORE.getIndexes()[Constants.ITEM_DEFINITIONS_INDEX].getFile(getArchiveId(), getFileId()); 		if (data == null) { 		 			return; 		} 		readOpcodeValues(new InputStream(data)); 		if (certTemplateId != -1) 			toNote(); 		if (lendTemplateId != -1) 			toLend(); 		if (unknownValue1 != -1) 			toBind(); 		loaded = true; 	} 	 	 	 	public void toNote() { 		 		ItemDefinitions realItem = getItemDefinitions(certId); 		membersOnly = realItem.membersOnly; 		value = realItem.value; 		name = realItem.name; 		stackable = 1; 		noted = true; 	}  	public void toBind() { 		 		ItemDefinitions realItem = getItemDefinitions(unknownValue2); 		originalModelColors = realItem.originalModelColors; 		maleEquipModelId3 = realItem.maleEquipModelId3; 		femaleEquipModelId3 = realItem.femaleEquipModelId3; 		teamId = realItem.teamId; 		value = 0; 		membersOnly = realItem.membersOnly; 		name = realItem.name; 		inventoryOptions = new String[5]; 		groundOptions = realItem.groundOptions; 		if (realItem.inventoryOptions != null) 			for (int optionIndex = 0; optionIndex &lt; 4; optionIndex++) 				inventoryOptions[optionIndex] = realItem.inventoryOptions[optionIndex]; 		inventoryOptions[4] = "Discard"; 		maleEquip1 = realItem.maleEquip1; 		maleEquip2 = realItem.maleEquip2; 		femaleEquip1 = realItem.femaleEquip1; 		femaleEquip2 = realItem.femaleEquip2; 		clientScriptData = realItem.clientScriptData; 		equipSlot = realItem.equipSlot; 		equipType = realItem.equipType; 	}  	public void toLend() { 		// ItemDefinitions lendItem; //lendTemplateId 		ItemDefinitions realItem = getItemDefinitions(lendId); 		originalModelColors = realItem.originalModelColors; 		maleEquipModelId3 = realItem.maleEquipModelId3; 		femaleEquipModelId3 = realItem.femaleEquipModelId3; 		teamId = realItem.teamId; 		value = 0; 		membersOnly = realItem.membersOnly; 		name = realItem.name; 		inventoryOptions = new String[5]; 		groundOptions = realItem.groundOptions; 		if (realItem.inventoryOptions != null) 			for (int optionIndex = 0; optionIndex &lt; 4; optionIndex++) 				inventoryOptions[optionIndex] = realItem.inventoryOptions[optionIndex]; 		inventoryOptions[4] = "Discard"; 		maleEquip1 = realItem.maleEquip1; 		maleEquip2 = realItem.maleEquip2; 		femaleEquip1 = realItem.femaleEquip1; 		femaleEquip2 = realItem.femaleEquip2; 		clientScriptData = realItem.clientScriptData; 		equipSlot = realItem.equipSlot; 		equipType = realItem.equipType; 		lended = true; 	}  	public int getArchiveId() { 		return getId() >>> 8; 	}  	public int getFileId() { 		return 0xff &amp; getId(); 	}  	public boolean isDestroyItem() { 		if (inventoryOptions == null) 			return false; 		for (String option : inventoryOptions) { 			if (option == null) 				continue; 			if (option.equalsIgnoreCase("destroy")) 				return true; 		} 		return false; 	}  	public boolean containsOption(int i, String option) { 		if (inventoryOptions == null || inventoryOptions[i] == null 				|| inventoryOptions.length &lt;= i) 			return false; 		return inventoryOptions[i].equals(option); 	}  	public boolean containsOption(String option) { 		if (inventoryOptions == null) 			return false; 		for (String o : inventoryOptions) { 			if (o == null || !o.equals(option)) 				continue; 			return true; 		} 		return false; 	}  	public boolean isWearItem() { 		return equipSlot != -1; 	}  	public boolean isWearItem(boolean male) { 		if (equipSlot &lt; Equipment.SLOT_RING 				&amp;&amp; (male ? getMaleWornModelId1() == -1 				: getFemaleWornModelId1() == -1)) 			return false; 		return equipSlot != -1; 	}  	public boolean hasSpecialBar() { 		if (clientScriptData == null) 			return false; 		Object specialBar = clientScriptData.get(686); 		if (specialBar != null &amp;&amp; specialBar instanceof Integer) 			return (Integer) specialBar == 1; 		return false; 	} 	 	public int getAttackSpeed() { 		if (clientScriptData == null) 			return 4; 		Object attackSpeed = clientScriptData.get(14); 		if (attackSpeed != null &amp;&amp; attackSpeed instanceof Integer) 			return (int) attackSpeed; 		return 4; 	} 	 	public int getStabAttack() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(0); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getSlashAttack() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(1); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getCrushAttack() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(2); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getMagicAttack() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(3); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getRangeAttack() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(4); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getStabDef() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(5); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getSlashDef() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(6); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getCrushDef() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(7); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getMagicDef() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(8); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getRangeDef() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(9); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getSummoningDef() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(417); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getAbsorveMeleeBonus() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(967); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getAbsorveMageBonus() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(969); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getAbsorveRangeBonus() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(968); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getStrengthBonus() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(641); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value / 10; 		return 0; 	} 	 	public int getRangedStrBonus() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(643); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value / 10; 		return 0; 	} 	 	public int getMagicDamage() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(685); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	} 	 	public int getPrayerBonus() { 		if (clientScriptData == null) 			return 0; 		Object value = clientScriptData.get(11); 		if (value != null &amp;&amp; value instanceof Integer) 			return (int) value; 		return 0; 	}   	public int getRenderAnimId() { 		if (clientScriptData == null) 			return 1426; 		Object animId = clientScriptData.get(644); 		if (animId != null &amp;&amp; animId instanceof Integer) 			return (Integer) animId; 		return 1426; 	}  	public int getModelZoom() { 		return modelZoom; 	}  	public int getModelOffset1() { 		return modelOffset1; 	}  	public int getModelOffset2() { 		return modelOffset2; 	}  	public int getQuestId() { 		if (clientScriptData == null) 			return -1; 		Object questId = clientScriptData.get(861); 		if (questId != null &amp;&amp; questId instanceof Integer) 			return (Integer) questId; 		return -1; 	}  	public List&lt;Item> getCreateItemRequirements() { 		if (clientScriptData == null) 			return null; 		List&lt;Item> items = new ArrayList&lt;Item>(); 		int requiredId = -1; 		int requiredAmount = -1; 		for (int key : clientScriptData.keySet()) { 			Object value = clientScriptData.get(key); 			if (value instanceof String) 				continue; 			if (key >= 538 &amp;&amp; key &lt;= 770) { 				if (key % 2 == 0) 					requiredId = (Integer) value; 				else 					requiredAmount = (Integer) value; 				if (requiredId != -1 &amp;&amp; requiredAmount != -1) { 					items.add(new Item(requiredId, requiredAmount)); 					requiredId = -1; 					requiredAmount = -1; 				} 			} 		} 		return items; 	}  	public HashMap&lt;Integer, Object> getClientScriptData() { 		return clientScriptData; 	}  	public HashMap&lt;Integer, Integer> getWearingSkillRequiriments() { 		if (clientScriptData == null) 			return null; 		if (itemRequiriments == null) { 			HashMap&lt;Integer, Integer> skills = new HashMap&lt;Integer, Integer>(); 			for (int i = 0; i &lt; 10; i++) { 				Integer skill = (Integer) clientScriptData.get(749 + (i * 2)); 				if (skill != null) { 					Integer level = (Integer) clientScriptData.get(750 + (i * 2)); 					if (level != null) 						skills.put(skill, level); 				} 			} 			Integer maxedSkill = (Integer) clientScriptData.get(277); 			if (maxedSkill != null) 				skills.put(maxedSkill, getId() == 19709 ? 120 : 99); 			itemRequiriments = skills; 			if (getId() == 7462) 				itemRequiriments.put(Skills.DEFENCE, 40); 			else if (getId() == 19784 || getId() == 22401 || getId() == 19780) { // Korasi 				itemRequiriments.put(Skills.ATTACK, 78); 				itemRequiriments.put(Skills.STRENGTH, 78); 			} else if (getId() == 20822 || getId() == 20823 || getId() == 20824 					|| getId() == 20825 || getId() == 20826) 				itemRequiriments.put(Skills.DEFENCE, 99); 			else if (name.equals("Dragon defender")) { 				itemRequiriments.put(Skills.ATTACK, 60); 				itemRequiriments.put(Skills.DEFENCE, 60); 			} 		} 		return itemRequiriments; 	}  	public void setDefaultOptions() { 		groundOptions = new String[] { null, null, "take", null, null }; 		inventoryOptions = new String[] { null, null, null, null, "drop" }; 	}  	public void setDefaultsVariableValues() { 		name = "null"; 		maleEquip1 = -1; 		maleEquip2 = -1; 		femaleEquip1 = -1; 		femaleEquip2 = -1; 		modelZoom = 2000; 		lendId = -1; 		lendTemplateId = -1; 		certId = -1; 		certTemplateId = -1; 		unknownInt9 = 128; 		value = 1; 		maleEquipModelId3 = -1; 		femaleEquipModelId3 = -1; 		unknownValue1 = -1; 		unknownValue2 = -1; 		teamId = -1; 		equipSlot = -1; 		equipType = -1; 	}  	public final void readValues(InputStream stream, int opcode) { 		if (opcode == 1) 			modelId = stream.readBigSmart(); 		else if (opcode == 2) 			name = stream.readString(); 		else if (opcode == 4) 			modelZoom = stream.readUnsignedShort(); 		else if (opcode == 5) 			modelRotation1 = stream.readUnsignedShort(); 		else if (opcode == 6) 			modelRotation2 = stream.readUnsignedShort(); 		else if (opcode == 7) { 			modelOffset1 = stream.readUnsignedShort(); 			if (modelOffset1 > 32767) 				modelOffset1 -= 65536; 			modelOffset1 &lt;&lt;= 0; 		} else if (opcode == 8) { 			modelOffset2 = stream.readUnsignedShort(); 			if (modelOffset2 > 32767) 				modelOffset2 -= 65536; 			modelOffset2 &lt;&lt;= 0; 		} else if (opcode == 11) 			stackable = 1; 		else if (opcode == 12) 			value = stream.readInt(); 		else if (opcode == 13) { 			equipSlot = stream.readUnsignedByte(); 		} else if (opcode == 14) { 			equipType = stream.readUnsignedByte(); 		} else if (opcode == 16) 			membersOnly = true; 		else if (opcode == 18) { 			stream.readUnsignedShort(); 		} else if (opcode == 23) 			maleEquip1 = stream.readBigSmart(); 		else if (opcode == 24) 			maleEquip2 = stream.readBigSmart(); 		else if (opcode == 25) 			femaleEquip1 = stream.readBigSmart(); 		else if (opcode == 26) 			femaleEquip2 = stream.readBigSmart(); 		else if (opcode == 27) 			stream.readUnsignedByte(); 		else if (opcode >= 30 &amp;&amp; opcode &lt; 35) 			groundOptions[opcode - 30] = stream.readString(); 		else if (opcode >= 35 &amp;&amp; opcode &lt; 40) 			inventoryOptions[opcode - 35] = stream.readString(); 		else if (opcode == 40) { 			int length = stream.readUnsignedByte(); 			originalModelColors = new int[length]; 			modifiedModelColors = new int[length]; 			for (int index = 0; index &lt; length; index++) { 				originalModelColors[index] = stream.readUnsignedShort(); 				modifiedModelColors[index] = stream.readUnsignedShort(); 			} 		} else if (opcode == 41) { 			int length = stream.readUnsignedByte(); 			originalTextureColors = new short[length]; 			modifiedTextureColors = new short[length]; 			for (int index = 0; index &lt; length; index++) { 				originalTextureColors[index] = (short) stream 						.readUnsignedShort(); 				modifiedTextureColors[index] = (short) stream 						.readUnsignedShort(); 			} 		} else if (opcode == 42) { 			int length = stream.readUnsignedByte(); 			unknownArray1 = new byte[length]; 			for (int index = 0; index &lt; length; index++) 				unknownArray1[index] = (byte) stream.readByte(); 		} else if (opcode == 44) { 			int length = stream.readUnsignedShort(); 			int arraySize = 0; 			for (int modifier = 0; modifier > 0; modifier++) { 				arraySize++; 				unknownArray3 = new byte[arraySize]; 				byte offset = 0; 				for (int index = 0; index &lt; arraySize; index++) { 					if ((length &amp; 1 &lt;&lt; index) > 0) { 						unknownArray3[index] = offset; 					} else { 						unknownArray3[index] = -1; 					} 				} 			} 		} else if (45 == opcode) { 			int i_97_ =(short) stream.readUnsignedShort(); 			int i_98_ = 0; 			for (int i_99_ = i_97_; i_99_ > 0; i_99_ >>= 1) 				i_98_++; 			unknownArray6 = new byte[i_98_]; 			byte i_100_ = 0; 			for (int i_101_ = 0; i_101_ &lt; i_98_; i_101_++) { 				if ((i_97_ &amp; 1 &lt;&lt; i_101_) > 0) { 					unknownArray6[i_101_] = i_100_; 					i_100_++; 				} else 					unknownArray6[i_101_] = (byte) -1; 			} 		} else if (opcode == 65) 			unnoted = true; 		else if (opcode == 78) 			maleEquipModelId3 = stream.readBigSmart(); 		else if (opcode == 79) 			femaleEquipModelId3 = stream.readBigSmart(); 		else if (opcode == 90) 			unknownInt1 = stream.readBigSmart(); 		else if (opcode == 91) 			unknownInt2 = stream.readBigSmart(); 		else if (opcode == 92) 			unknownInt3 = stream.readBigSmart(); 		else if (opcode == 93 || opcode == 94) 			unknownInt4 = stream.readBigSmart(); 		else if (opcode == 95) 			unknownInt5 = stream.readUnsignedShort(); 		else if (opcode == 96) 			unknownInt6 = stream.readUnsignedByte(); 		else if (opcode == 97) 			certId = stream.readUnsignedShort(); 		else if (opcode == 98) 			certTemplateId = stream.readUnsignedShort(); 		else if (opcode >= 100 &amp;&amp; opcode &lt; 110) { 			if (stackIds == null) { 				stackIds = new int[10]; 				stackAmounts = new int[10]; 			} 			stackIds[opcode - 100] = stream.readUnsignedShort(); 			stackAmounts[opcode - 100] = stream.readUnsignedShort(); 		} else if (opcode == 110) 			unknownInt7 = stream.readUnsignedShort(); 		else if (opcode == 111) 			unknownInt8 = stream.readUnsignedShort(); 		else if (opcode == 112) 			unknownInt9 = stream.readUnsignedShort(); 		else if (opcode == 113) 			unknownInt10 = stream.readByte(); 		else if (opcode == 114) 			unknownInt11 = stream.readByte() * 5; 		else if (opcode == 115) 			teamId = stream.readUnsignedByte(); 		else if (opcode == 121) 			lendId = stream.readUnsignedShort(); 		else if (opcode == 122) 			lendTemplateId = stream.readUnsignedShort(); 		else if (opcode == 125) { 			unknownInt12 = stream.readByte() &lt;&lt; 0; 			unknownInt13 = stream.readByte() &lt;&lt; 0; 			unknownInt14 = stream.readByte() &lt;&lt; 0; 		} else if (opcode == 126) { 			unknownInt15 = stream.readByte() &lt;&lt; 0; 			unknownInt16 = stream.readByte() &lt;&lt; 0; 			unknownInt17 = stream.readByte() &lt;&lt; 0; 		} else if (opcode == 127) { 			unknownInt18 = stream.readUnsignedByte(); 			unknownInt19 = stream.readUnsignedShort(); 		} else if (opcode == 128) { 			unknownInt20 = stream.readUnsignedByte(); 			unknownInt21 = stream.readUnsignedShort(); 		} else if (opcode == 129) { 			unknownInt20 = stream.readUnsignedByte(); 			unknownInt21 = stream.readUnsignedShort(); 		} else if (opcode == 130) { 			unknownInt22 = stream.readUnsignedByte(); 			unknownInt23 = stream.readUnsignedShort(); 		} else if (opcode == 132) { 			int length = stream.readUnsignedByte(); 			unknownArray2 = new int[length]; 			for (int index = 0; index &lt; length; index++) 				unknownArray2[index] = stream.readUnsignedShort(); 		} else if (opcode == 134) { 			int unknownValue = stream.readUnsignedByte(); 		} else if (opcode == 139) { 			unknownValue2 = stream.readUnsignedShort(); 		} else if (opcode == 140) { 			unknownValue1 = stream.readUnsignedShort(); 		} else if (opcode >= 142 &amp;&amp; opcode &lt; 147) { 			if (unknownArray4 == null) { 				unknownArray4 = new int[6]; 				Arrays.fill(unknownArray4, -1); 			} 			unknownArray4[opcode - 142] = stream.readUnsignedShort(); 		} else if (opcode >= 150 &amp;&amp; opcode &lt; 155) { 			if (null == unknownArray5) { 				unknownArray5 = new int[5]; 				Arrays.fill(unknownArray5, -1); 			} 			unknownArray5[opcode - 150] = stream.readUnsignedShort(); 		} else if (opcode == 142) { 			stream.readByte(); 			stream.readByte(); 		} else if (opcode == 144) { 			stream.readByte(); 			stream.readByte(); 			stream.readByte(); 		} else if (opcode == 151) { 			stream.readByte(); 			stream.readByte(); 		} else if (opcode == 249) { 			int length = stream.readUnsignedByte(); 			if (clientScriptData == null) 				clientScriptData = new HashMap&lt;Integer, Object>(length); 			for (int index = 0; index &lt; length; index++) { 				boolean stringInstance = stream.readUnsignedByte() == 1; 				int key = stream.read24BitInt(); 				Object value = stringInstance ? stream.readString() : stream 						.readInt(); 				clientScriptData.put(key, value); 			} 		} else 			throw new RuntimeException("MISSING OPCODE " + opcode 					+ " FOR ITEM " + getId()); 	}  	public int unknownValue1; 	public int unknownValue2;  	public final void readOpcodeValues(InputStream stream) { 		while (true) { 			int opcode = stream.readUnsignedByte(); 			if (opcode == 0) 				break; 			readValues(stream, opcode); 		} 	}  	public String getName() { 		return name; 	} 	public int getFemaleWornModelId1() { 		return femaleEquip1; 	} 	public int getFemaleWornModelId2() { 		return femaleEquip2; 	} 	public int getMaleWornModelId1() { 		return maleEquip1; 	} 	public int getMaleWornModelId2() { 		return maleEquip2; 	}  	public boolean isOverSized() { 		return modelZoom > 5000; 	}  	public boolean isLended() { 		return lended; 	} 	public boolean isMembersOnly() { 		return membersOnly; 	} public boolean isStackable() { return stackable == 1; } public boolean isNoted() { return noted; } public int getLendId() { 	return lendId; 	} public int getCertId() { 	return certId;}     public int getValue() {         return value;     }   public void setValue(int value) { this.value = value; }  public int getId() { return id; }  public int getEquipSlot() { return equipSlot; }  public int getEquipType() { return equipType; 	} }
