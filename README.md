local mhyesTOGGLE = false
local maxfpsTOGGLE = false
local fixgrassTOGGLE = false
local hidenametwoTOGGLE = false
local upgradefpsTOGGLE = false
local dropviewTOGGLE = false
local mhnovtwoTOGGLE = false
local bypasslobTOGGLE = false
local bypasscheatTOGGLE = false
local bypassreportTOGGLE = false
local bypasslobbyTOGGLE = false


local libTarget = gg.getRangesList("liblogic.so")

local strA = 0x0
local strB = 0x0
local strC = 0x0
local strD = 0x0
local strE = 0x0
local strF = 0x0

local h1 = 0x0
local h2 = 0x0
local h3 = 0x0
local h4 = 0x0
local h5 = 0x0
local h6 = 0x0

local hx1 = 0x0
local hx2 = 0x0
local hx3 = 0x0
local hx4 = 0x0
local hx5 = 0x0
local hx6 = 0x0
-- script creator: Nok1a (before under the name Platonic/Mr.Dragon Star)
-- Tutorial on how to use:  https://youtu.be/OylUtyEwGVA
-- Use whatever you like in the script in case needed
-- value = Time.timeScale

local hexConvert = 0x0
local dataType = 0x0
local cdOffsetSmall = 0x0
local cdOffsetBig =  0x0
local cbOffsetSmall = 0x0
local anonSpeedOffsetSmall = 0x0
local anonSpeedOffsetBig = 0x0
local anonGroupSearchOffset = 0x0

local game = gg.getTargetInfo() or {x64 = gg.alert('Is game?', 'x64', '', 'x32') == 1}
local x64 = game.x64
local ggg = {}
for a, b in next, gg do
	ggg[a] = b
end
local cfg = loadfile(gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$'))
cfg = cfg and cfg().version == version and cfg() or {-77, 47, false, false}
local function ml()
	local a = gg.prompt({
		'Camera view A: <Plus Horizontal>[-82;-50]',
		'Camera view B: <Just Vertical>[0;99]',
		'Memory ranges',
		'On Hack ≥ 50% Loading'
	},
	cfg,
	{'number', 'number', 'checkbox', 'checkbox', 'checkbox', 'checkbox'})
	if not a then
		gg.setVisible(false)
		return
	end
	if a[3] then
		local r = {
			'REGION_JAVA_HEAP',
			'REGION_C_HEAP',
			'REGION_C_ALLOC',
			'REGION_C_DATA',
			'REGION_C_BSS',
			'REGION_PPSSPP',
			'REGION_ANONYMOUS',
			'REGION_JAVA',
			'REGION_STACK',
			'REGION_ASHMEM',
			'REGION_VIDEO',
			'REGION_OTHER',
			'REGION_BAD',
			'REGION_CODE_APP',
			'REGION_CODE_SYS'
		}
		local m = {
			'Jh: Java heap',
			'Ch: C++ heap',
			'Ca: C++ Alloc',
			'Cd: C++ .data',
			'Cb: C++ .bss',
			'PS: PPSSPP',
			'A: Anonymous',
			'J: Java',
			'S: Stack',
			'As: Ashmem',
			'V: Video',
			'O: Other',
			'B: Bad',
			'Xa: Code App',
			'Xs: Code system',
		}
		local s = {}
		for z, x in next, gg.getRangesList() do
			s[x.state] = s[x.state] or x['end'] - x.start
			s[x.state] = s[x.state] + (x['end'] - x.start)
		end
		local u = {}
		for z, x in next, s do
			u.GB = u.GB or {}
			u.GB[z] = x / 1000000000
			u.MB = u.MB or {}
			u.MB[z] = (x / 1000000000) == 0 and x / 1000000 or nil
			u.KB = u.KB or {}
			u.KB[z] = (x / 1000000) == 0 and x / 1000 or nil
		end
		for z, x in next, u do
			for q, w in next, x do
				if w > 0 then
					for o, p in next, m do
						if p:match(q .. ':') then
							m[o] = p .. ' [' .. w .. ' ' .. z .. ']'
						end
					end
				end
			end
		end
		local d = {}
		for z, x in next, r do
			d[z] = gg[x] ~= 0
		end
		local c = gg.multiChoice(m, d, 'MEMORY RANGES:')
		if c then
			for z, x in next, c do
				gg[r[z]] = ggg[r[z]]
				r[z] = nil
			end
			for z, x in next, r do
				gg[r[z]] = 0
			end
			gg.saveVariable(r, gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$') .. '.ranges')
		end
		return ml()
	end
	a[3] = false
	a.version = version
	gg.saveVariable(a, gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$'))
	for z, x in next, a do
		cfg[z] = x
	end
	if a[1] ~= '0' then
		a[1] = a[1] .. '000000'
		a[1] = '-1000130432' + a[1]
	end
	if a[2] ~= '0' then
		a[2] = a[2] .. '000000'
		a[2] = '1000130432' + (a[2] * 2)
	end
		if a[6] then--Hide name
			gg.setRanges(gg.REGION_C_ALLOC | gg.REGION_OTHER | gg.REGION_ANONYMOUS)
			gg.clearResults()
			gg.searchNumber(game.x64 and "1,157,234,688" or '4,225', 4)
			local found = gg.getResults(gg.getResultsCount())
			gg.clearResults()
			local orval = {'1082130432'}
			for a, b in next, orval do
				orval[a] = b + 0
			end
			local val = {}
			for _, __ in next, found do 
				local c = gg.getValues({
					{address = __.address - (4 * 11), flags = 4, value = 0},
					{address = __.address - (4 * 12), flags = 4, value = 0}
				})
				if c[1].value == 2 then k() end
				if c[1].value == "1093664768" + 0  then
					if c[2].value == orval[1] then
						c[2].value = -1
					elseif c[2].value == -1 then
						c[2].value = orval[1]
					end
					c[2].freeze = true
					val[#val + 1] = c[2]
				end
			end
			for _, __ in next, val do
				gg.addListItems(val)
				gg.removeListItems(val)
				if __.value == -1 then
					gg.toast("Success Hack.")
				else
					gg.toast("Success revert.")
				end
				break
			end
		end
	if true then
		local val = loadfile(gg.EXT_CACHE_DIR .. '/val.lua')
		val = val and val()[16] and val()
		for z, x in next, gg.getValues(val or {}) do
			val = val and x.value == val[z].value and val or nil
		end
		gg.setRanges(gg.REGION_ANONYMOUS)
		gg.clearResults(gg.setVisible(false))
		gg.clearResults()
		val = not val and gg.searchNumber('h0E000000170000000B00000001000000', gg.TYPE_BYTE)--14;256;23;4,294,967,307Q
		local res = gg.getResults(gg.getResultsCount())
		gg.clearResults() 
		for z, x in next, res do
			res[1].value = 9
			res[1].freeze = true
			gg.addListItems(res)
			gg.removeListItems(res)
			gg.saveVariable(res, gg.EXT_CACHE_DIR .. '/val.lua')
			gg.toast('Xray Actived')
		end
		gg.setRanges(gg.REGION_C_ALLOC | gg.REGION_OTHER)
		gg.clearResults()
		gg.searchNumber(x64 and '19,864,223,744,017' or '4,225', x64 and gg.TYPE_QWORD or 4)--1081006571
		local res = gg.getResults(gg.getResultsCount())
		gg.clearResults()
		local f = {}
		for z, x in next, res do
			f[#f + 1] = {address = x.address + (x64 and 0xD0 or 0xB0), flags = 4, value = x.value}
		end
		f = gg.getValues(f)
		local e = {}
		local p = {{}, {-1082130264, -1082129594, -1082128755}}
		for z, x in next, p[2] do
			p[1][x] = z
		end
		p = p[1]
		for z, x in next, f do
			if p[x.value] then
				e[#e + 1] = {address = x.address + 0x4, flags = 4, value = a[1], freeze = true}
				e[#e + 1] = {address = x.address + 0x14, flags = 4, value = a[2], freeze = true}
				gg.addListItems(e)
				gg.removeListItems(e)
				gg.toast('Success hack')
			end
		end
		if a[4] then
			local s = os.time()
			while #e > 0 and os.time() - s < 181 do
				if gg.isVisible() then
					gg.setVisible(false)
					gg.toast('Acces not permitted.')
				end
				for z, x in next, gg.getValues(e) do
					if x.value == 0 then
						gg.addListItems(e)
						z = os.time()
						while os.time() - z < 6 do
							gg.setVisible(false)
						end
						gg.removeListItems(e)
						gg.toast('Script ended')
						e = {}
						break
					end
				end
			end
		end
	end
	local b = os.time()
	while not gg.isVisible() do
		if os.time() - b > 2 then
			return
		end
	end
	return ml()
end
local b = loadfile(gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$') .. '.ranges')
b = b and b() or {}
for z, x in next, b do
	gg[x] = 0
end

function checkArchitecture()
    local targetInfo = gg.getTargetInfo()
    local arch = gg.getTargetInfo().x64
    gg.alert("ITACHI MLBB LOADER BETA TEST")
    if arch then
        gg.alert("You Are Running On 64Bit")
        onA = "-2,999,674,700,646,286,368"
    else
        gg.alert("You Are Running On 32Bit")
        onA = "-2,220,275,583,130,009,600"
    end
end

function isProcess64Bit()
  -- Function -> by CmP: https://gameguardian.net/forum/topic/36604-how-to-get-instruction-set-architecture-on-emulator-virtual-memory-addresses/?do=findComment&comment=135506
  local regions = gg.getRangesList()
  local lastAddress = regions[#regions]["end"]
  return (lastAddress >> 32) ~= 0
end

function noselect()
  gg.toast("You not select anything")
end

function cb_nothingFound()
  if gg.getResultsCount() == 0 then
    gg.alert("Search not found, Use Fuzzy or Xa")
    start()
  end
end


local ISA = isProcess64Bit()
local resultsCount = gg.getResultsCount()
gg.clearResults()

function instructions()
  if ISA == false then -- if true then 32 bit else 64 bit
    hexConvert = 0x100000000
    dataType = 4
    cdOffsetSmall = 0xC
    cdOffsetBig =  0xB58
    cbOffsetSmall = 0x1C
    anonSpeedOffsetSmall = 0xE4
    anonSpeedOffsetBig = 0xEC
    anonGroupSearchOffset = 0x30
  else
    hexConvert = 0x0
    dataType = 32
    cdOffsetSmall = 0x18
    cdOffsetBig = 0x16B0
    cbOffsetSmall = 0x38
    cbOffsetBig = 0x48
    anonSpeedOffsetSmall = 0xF4
    anonSpeedOffsetBig = 0xFC
    anonGroupSearchOffset = 0x44
  end
end
function unityVersion()
  gg.searchNumber(":Expected version: ", gg.TYPE_BYTE, gg.setRanges(16384))
  if gg.getResultsCount() == 0 then
    gg.alert("This game is not made with Unity, fuzzy will be automatically activated!")
    gg.alert("Make sure your game is open, don't hide it on the background!")
    randomized()
    os.exit()
  else
    local count = gg.getResultsCount()
    local results = gg.getResults(count)
    values = {}
    for i = 18, count, 18 do
      local resultAddress = results[i].address
      for j = 1, 7 do
        values[#values + 1] = {address = resultAddress + j, flags = gg.TYPE_BYTE}
      end
      values = gg.getValues(values)
    end
    
    local str = ''
    for i, v in ipairs(values) do
      str = str .. string.char(v.value & 0xFF)
    end
    gg.clearResults()
    if values[1].value <= 0x35 and values[2].value == 0x2E and values[3].value <= 0x35 then -- unity versions 5.5 or below
      gg.alert("Unity version "..str..",This version is to old. Use Speedhack through Cb or Fuzzy")
      anonGroupSearchOffset = 0x3C
      cbOffsetSmall = 0x24
      anonSpeedOffsetSmall = 0xBC
    end
    gg.clearResults()
  end
end


function startxa()
  gg.clearResults()
  gg.setRanges(gg.REGION_CODE_APP)
  gg.searchNumber("h 6A 6F 79 73 74 69 63 6B 20 31 36 20 62 75 74 74 6F 6E 20 31 39")
  if gg.getResultsCount() == 0 then
    gg.alert("Search not found, try Speedhack Cb or Fuzzy")
    start()
  else
    local a = gg.getResults(1)
    gg.clearResults()
    gg.setRanges(gg.REGION_C_DATA | gg.REGION_OTHER)
    gg.searchNumber(a[1].address, dataType)
    resultsCount = gg.getResultsCount()
    a = gg.getResults(1)
    gg.clearResults()
    gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
    gg.searchNumber(a[1].address + cdOffsetSmall, dataType)
    if gg.getResultsCount() ~= 1 then 
      gg.clearResults()
      gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
      gg.searchNumber(a[1].address - cdOffsetBig, dataType)
    end
    finalSpeedResult()
  end
end

function filterCb()
  local b = {}
  gg.clearResults()
  gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
  gg.searchNumber("h 00 00 80 3F CD CC 4C 3E 00 00 00 41 00 00 C8 42 00 00 B4 43 0A D7 23 3C CD CC 4C 3E 00 00 40 3F 00 00 00")
  if gg.getResultsCount() == 0 then
    gg.searchNumber("-9.81000041962", gg.TYPE_FLOAT)
    cb_nothingFound()
    resultsCount = gg.getResultsCount()
    local t = gg.getResults(resultsCount)
    for i, v in ipairs(t) do
      v.address = v.address + 0xC
    end
    gg.loadResults(t)
    gg.refineNumber("1", gg.TYPE_FLOAT)
    cb_nothingFound()
    resultsCount = gg.getResultsCount()
    b = gg.getResults(resultsCount)
  elseif gg.getResultsCount() ~= 0 then
    gg.refineNumber("h 00 00 80 3F", gg.TYPE_FLOAT)
    cb_nothingFound()
    resultsCount = gg.getResultsCount()
    local a = gg.getResults(resultsCount)
    for i, v in ipairs(a) do
      v.flags = gg.TYPE_FLOAT
    end
    b = gg.getValues(a)
  end
  return b
end
function startcb()
  local fakeSpeedPointers = {}
  local b = filterCb()
  for i, v in ipairs(b) do
    if v.value == 1 then
      fakeSpeedPointers[#fakeSpeedPointers + 1] = {address = v.address - anonGroupSearchOffset, flags = gg.TYPE_FLOAT}
      fakeSpeedPointers[#fakeSpeedPointers + 1] = {address = v.address - 0x48, flags = gg.TYPE_FLOAT}
      fakeSpeedPointers[#fakeSpeedPointers + 1] = {address = v.address - 0x44, flags = gg.TYPE_FLOAT}
    end
  end
  gg.setRanges(gg.REGION_C_BSS | gg.REGION_OTHER | gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC)
  gg.loadResults(fakeSpeedPointers)
  gg.searchPointer(0)
  resultsCount = gg.getResultsCount()
  local offsetStart = gg.getResults(resultsCount)
  for i, v in ipairs(offsetStart) do
    v.address = v.address - cbOffsetSmall
  end
  local valu = gg.getValues(offsetStart)
  for i, v in ipairs(valu) do
    if ISA == false then
      v.address = v.value&0xFFFFFFFF
    else
      v.address = v.value
    end
  end
  gg.loadResults(valu)
  finalSpeedResult()
end


function randomized()
  gg.clearResults()
  gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
  gg.searchFuzzy('0', gg.TYPE_FLOAT, gg.SIGN_FUZZY_GREATER)
  for i = 1 , 50 do
    gg.searchFuzzy('0', gg.TYPE_FLOAT, gg.SIGN_FUZZY_GREATER)
    gg.refineNumber('0.01~6', gg.TYPE_FLOAT)
    gg.sleep(50)                                                   
  end
  resultsCount = gg.getResultsCount()
  secondResults = gg.getResults(resultsCount)
  local addTables = {}
  local speed = {}
  for i = 1, #secondResults do
    local loops = 0x0
    for b = 1, 200 do
      addTables[#addTables + 1] = {address = secondResults[i].address + loops, flags = gg.TYPE_FLOAT}
      addTables[#addTables + 1] = {address = secondResults[i].address - loops, flags = gg.TYPE_FLOAT}
      loops = loops + 0x4
    end
  end
  addTables = gg.getValues(addTables)
  for i, v in ipairs (addTables) do
    if v.value == 1 then
      speed[#speed +1] = {address = v.address, flags = v.flags, name = "Speed"}
    end
  end
  if #speed ~= 0 then
    gg.toast("Possible speed values found(not guaranteed)")
    gg.addListItems(speed)
    gg.clearResults()
  else
    gg.toast("Nothing found with Fuzzy, shuttind down script")
  end
  os.exit()
end

local functLoop = true
function start()
  if functLoop == true then
    instructions()
    unityVersion()
    functLoop = false
  end
end

--------------------------------bypass menu-----------------------------------------
function bypasscheatON() --choice
bypasscheatTOGGLE = false
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x5c1539c, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x5c1539c + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x2f9d390, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x2f9d390 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x2f9d394, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x2f9d394 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x5c15398, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x5c15398 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x57EBF28, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x57EBF28 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x57E7D90, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x57E7D90 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D600, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D600 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D608, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D608 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D618, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D618 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D620, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D620 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D630, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D630 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D638, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D638 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D640, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D640 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D648, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D648 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D650, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D650 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D660, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D660 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D668, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D668 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D678, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D678 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D680, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D680 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D690, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D690 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D698, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D698 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D6A8, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D6A8 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D6B0, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D6B0 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D6C0, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D6C0 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D6C8, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D6C8 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
local LibStart = gg.getRangesList('liblogic.so')[1].start
local values = {
    { address = LibStart + 0x12D6D8, value = 'h C0 03 5F D6', flags = 4 },
    { address = LibStart + (0x12D6D8 + 0x4), value = 'h C0 03 5F D6', flags = 4 }
}
for _, v in ipairs(values) do
    gg.setValues({ v })
end
gg.alert("𝐀𝐍𝐓𝐈 𝐂𝐇𝐄𝐀𝐓 𝐀𝐂𝐓𝐈𝐕𝐀𝐓𝐄")
end

function bypassreportON()
bypassreportTOGGLE = false
HexPatches.ITACHI("libnativelogin.so",0xb9e3c,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0xba1e4,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0xba2ac,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0x2b66c,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0x2b6ec,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0x2b7c4,"h00 00 80 D2 C0 03 5F D6",32);
gg.alert("𝐀𝐍𝐓𝐈 𝐂𝐇𝐄𝐀𝐓 𝐀𝐂𝐓𝐈𝐕𝐀𝐓𝐄")
end

-----------------------hack menu--------------------------
function droneON()
dropviewTOGGLE = false
function 
split(szFullString, szSeparator) 
local nFindStartIndex = 1 local nSplitIndex = 1 local nSplitArray 
= {} while true do local nFindLastIndex = string.find 
(szFullString, szSeparator, nFindStartIndex) 
if not nFindLastIndex then nSplitArray[nSplitIndex]
 = string.sub(szFullString, nFindStartIndex, string.len (szFullString))
 break end nSplitArray[nSplitIndex]
 = string.sub (szFullString, nFindStartIndex, nFindLastIndex - 1) nFindStartIndex 
= nFindLastIndex + string.len (szSeparator) nSplitIndex 
= nSplitIndex + 1 end return 
nSplitArray end function 
xgxc(szpy, qmxg) for x = 1, #(qmxg) do xgpy = szpy + qmxg[x]["offset"] xglx 
= qmxg[x]["type"] xgsz = qmxg[x]["value"] xgdj = qmxg[x]["freeze"] if xgdj 
== nil or xgdj == "" then gg.setValues({[1] = {address = xgpy, flags = xglx, value 
= xgsz}}) else gg.addListItems({[1] = {address = xgpy, flags = xglx, freeze
 = xgdj, value = xgsz}}) end xgsl = xgsl + 1 xgjg = true end end function
 xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList() gg.clearResults() gg.setRanges(qmnb[1]["memory"])
 gg.searchNumber(qmnb[3]["value"], qmnb[3]["type"]) if gg.getResultCount() 
== 0 then else gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"]) gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"]) gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"]) if gg.getResultCount() == 0 then else sl = gg.getResults(999999) sz = gg.getResultCount() xgsl = 0 if sz > 999999 then sz = 999999 end for i = 1, sz do pdsz = true for v = 4, #(qmnb) do if pdsz == true then pysz = {} pysz[1] = {} pysz[1].address = sl[i].address + qmnb[v]["offset"] pysz[1].flags = qmnb[v]["type"] szpy = gg.getValues(pysz) pdpd = qmnb[v]["lv"] .. ";" .. szpy[1].value szpd = split(pdpd, ";") tzszpd = szpd[1] pyszpd = szpd[2] if tzszpd == pyszpd then pdjg = true pdsz = true else pdjg = false pdsz = false end end end if pdjg == true then szpy = sl[i].address xgxc(szpy, qmxg) end end if xgjg == true then else end end end end function SearchWrite(Search, Write, Type) gg.clearResults() gg.setVisible(false) gg.searchNumber(Search[1][1], Type) local count = gg.getResultCount() local result = gg.getResults(count) gg.clearResults() local data = {} local base = Search[1][2] if (count > 0) then for i, v in ipairs(result) do v.isUseful = true end for k=2, #Search do local tmp = {} local offset = Search[k][2] - base local num = Search[k][1] for i, v in ipairs(result) do tmp[#tmp+1] = {} tmp[#tmp].address = v.address + offset tmp[#tmp].flags = v.flags end tmp = gg.getValues(tmp) for i, v in ipairs(tmp) do if ( tostring(v.value) ~= tostring(num) ) then result[i].isUseful = false end end end for i, v in ipairs(result) do if (v.isUseful) then data[#data+1] = v.address end end if (#data > 0) then local t = {} local base = Search[1][2] for i=1, #data do for k, w in ipairs(Write) do offset = w[2] - base t[#t+1] = {} t[#t].address = data[i] + offset t[#t].flags = Type t[#t].value = w[1] if (w[3] == true) then local item = {} item[#item+1] = t[#t] item[#item].freeze = true gg.addListItems(item) end end end gg.setValues(t) gg.addListItems(t) else return false end else return false end end

UBED = gg.prompt({
" \nᴅʀᴏɴᴇ ᴠɪᴇᴡ [0;12]", 
"ʙᴀᴄᴋ", 
},{
},{
"number",
"checkbox", 
})

if UBED == nil then else
if UBED[1] == "0" then
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "1" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1084227584",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "2" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1092616192",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "3" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1095712192",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "4" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1097859072",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "5" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1101004800",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "6" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1103626240",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "7" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1106247680",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "8" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1108082688",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "9" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1109393408",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "10" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1110704128",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "11" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1112014848",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "12" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1112512598",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "1" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1080452710",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "2" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1078774989",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "3" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1077097267",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "4" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1075419546",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "5" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1073741824",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "6" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1072902963",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "7" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1072064102",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "8" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1071225242",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "10" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1070386381",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "11" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1069547520",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "12" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1060426515",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
end

if UBED[2] == true then return menu end end HOMEDM = -1 end

function mhyesON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"CanSight",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 1
        h1 = "0x"..v.Offset..""
       end
       end
    local libTarget = gg.getRangesList("liblogic.so")

    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local a = h1
            local address = name.start + a
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx1 = (tostring(currentValue[1].value))
            end
            end
    mhyesTOGGLE = true
    gg.toast("Maphack Enabled")
    local a = h1
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end


function mhyesOFF()
    mhyesTOGGLE = false
    gg.toast("Maphack Disabled")
    local a = h1
    local aa = hx1
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = aa}
            })
            break
        end
    end
end

function mhnoON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"UpdateEyeLayer",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h2 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local b = h2
            local address = name.start + b
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx2 = (tostring(currentValue[1].value))
            end
            end
    mhnoTOGGLE = true
    gg.toast("Maphack No Icon Enabled")
    local b = h2
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function mhnoOFF()
    mhnoTOGGLE = false
    gg.toast("Maphack No Icon Disabled")
    local b = h2
    local bb = hx2
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = bb}
            })
            break
        end
    end
end

function mhnovtwoON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"CanSight",
"UpdateEyeLayer",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h1 = "0x"..v.Offset..""
       end
       end
for k ,v in ipairs(search[2]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h2 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local a = h1
            local address = name.start + a
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx1 = (tostring(currentValue[1].value))
            end
            end
            for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local b = h2
            local address = name.start + b
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx2 = (tostring(currentValue[1].value))
            end
            end
    mhnovtwoTOGGLE = true
    local a = h1
    local b = h2
    local bba = hx1
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = onA}
            })
            gg.sleep(1000)
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = onA}
            })
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = bba}
            })
            break
        end
    end
end

function mhnovtwoOFF()
    mhnovtwoTOGGLE = false
    local b = h2
    local bba = hx2
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = bba}
            })
            break
        end
    end
end

function fixgrassON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"SetGrassLayer",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h3 = "0x"..v.Offset..""
       end
       end
for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local c = h3
            local address = name.start + c
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx3 = (tostring(currentValue[1].value))
            end
            end
    fixgrassTOGGLEtwo = true
    gg.toast("Fix Grass Enabled")
    local c = h3
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + c, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function fixgrassOFF()
    fixgrassTOGGLEtwo = false
    gg.toast("Fix Grass Disabled")
    local c = h3
    local cc = hx3
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + c, flags = gg.TYPE_QWORD, value = cc}
            })
            break
        end
    end
end

function maxfpsON()
upgradefpsTOGGLE = true
for _FORV_8_, _FORV_9_ in ipairs({56277916, 56278756, 56279172, 56277200, 56279652, 56280572, 56280740}) do
gg.setValues({[1] = {address = gg.getRangesList("liblogic.so")[1].start + _FORV_9_, value = "h200080D2", flags = 4}, [2] = {address = gg.getRangesList("liblogic.so")[1].start + _FORV_9_ + 4, value = "hC0035FD6", flags = 4}})
gg.clearResults()
gg.toast("𝗠𝗔𝗫 𝗥𝗘𝗙𝗥𝗘𝗦𝗛 𝗥𝗔𝗧𝗘 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
end
end

function maxfpsOFF()
    maxfpsTOGGLE = false
    gg.toast("Upgrade Fps Disabled")
    local c = h3
    local cc = hx3
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + c, flags = gg.TYPE_QWORD, value = cc}
            })
            break
        end
    end
end

function hidenametwoON()
hidenametwoTOGGLE = true
gg.setRanges(gg.REGION_C_ALLOC)
gg.clearResults()
gg.searchNumber("4697254412429557760", gg.TYPE_QWORD)
gg.getResults(9999)
gg.editAll("-9999999", gg.TYPE_QWORD)
gg.clearResults()
gg.toast("𝗛𝗜𝗗𝗘 𝗡𝗔𝗠𝗘 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
gg.sleep("2000")
gg.clearResults()
end

function offnameOFF()
    maxfpsTOGGLE = false
    gg.toast("Hidename Disabled")
    local c = h3
    local cc = hx3
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + c, flags = gg.TYPE_QWORD, value = cc}
            })
            break
        end
    end
end

function lololo()
gg.alert("This Is For Resetting Banned Account Only")
gg.alert("Once Executed Restart All")
file = gg.FILES_DIR:gsub(gg.PACKAGE:gsub("%p", function(...)
        return "%" .. (...)
    end) .. ".-$", "", 1) .. "com.mobile.legends/shared_prefs/"
    data = io.open(file .. "com.mobile.legends.v2.playerprefs.xml", "r"):read("*all")
    data = data:gsub("\"JsonDeviceID\"%>(.-)%<", function(hex)
        local Hex = {}
        for i = 1, 5 do
            if i == 1 then
                local str = ""
                for i = 1, 56 / 2 do
                    str = str .. string.format("%02x", math.random(0,255))
                end
                Hex[#Hex + 1] = str
            elseif i == 2 or i == 3 or i == 4 then
                local str = ""
                for i = 1, 2 do
                    str = str .. string.format("%02x", math.random(0,255))
                end
                Hex[#Hex + 1] = str  
            elseif i == 5 then      
                local str = ""
                for i = 1, 6 do
                    str = str .. string.format("%02x", math.random(0,255))
                end
                Hex[#Hex + 1] = str
            end
        end
        print(hex)
        return "\"JsonDeviceID\">" .. table.concat(Hex,"-") .. "<"
    end,1)
    io.open(file .. "com.mobile.legends.v2.playerprefs.xml", "w"):write(data):close()
    gg.toast("Reset Account Success")
    end
    
    
function bypasscheat()
    if bypasscheatTOGGLE then
        return "🟢 𝗕𝗬𝗣𝗔𝗦𝗦 𝗔𝗡𝗧𝗜 𝗖𝗛𝗘𝗔𝗧"
    else
        return "🔴 𝗕𝗬𝗣𝗔𝗦𝗦 𝗔𝗡𝗧𝗜 𝗖𝗛𝗘𝗔𝗧"
    end
end 

function bypassreport()
    if bypassreportTOGGLE then
        return "🟢 𝗕𝗬𝗣𝗔𝗦𝗦 𝗔𝗡𝗧𝗜 𝗥𝗘𝗣𝗢𝗥𝗧"
    else
        return "🔴 𝗕𝗬𝗣𝗔𝗦𝗦 𝗔𝗡𝗧𝗜 𝗥𝗘𝗣𝗢𝗥𝗧"
    end
end

function bypasslobby()
    if bypasslobTOGGLE then
        return "🟢 𝗕𝗬𝗣𝗔𝗦𝗦 𝗟𝗢𝗕𝗕𝗬"
    else
        return "🔴 𝗕𝗬𝗣𝗔𝗦𝗦 𝗟𝗢𝗕𝗕𝗬"
    end
end
    
function drone()
    if dropviewTOGGLE then
        return "🟢 𝗗𝗥𝗢𝗡𝗘 𝗩𝗜𝗘𝗪"
    else
        return "🔴 𝗗𝗥𝗢𝗡𝗘 𝗩𝗜𝗘𝗪"
    end
end
    
function mhyes()
    if mhyesTOGGLE then
        return "🟢 𝗠𝗔𝗣𝗛𝗔𝗖𝗞"
    else
        return "🔴 𝗠𝗔𝗣𝗛𝗔𝗖𝗞"
    end
end

function mhnovtwo()
    if mhnovtwoTOGGLE then
        return "🟢 𝗠𝗔𝗣𝗛𝗔𝗖𝗞 𝗡𝗢 𝗜𝗖𝗢𝗡"
    else
        return "🔴 𝗠𝗔𝗣𝗛𝗔𝗖𝗞 𝗡𝗢 𝗜𝗖𝗢𝗡"
    end
end

function fixgrass()
    if fixgrassTOGGLE then
        return "🟢 𝗙𝗜𝗫 𝗚𝗥𝗔𝗦𝗦"
    else
        return "🔴 𝗙𝗜𝗫 𝗚𝗥𝗔𝗦𝗦"
    end
end

function hidenametwo()
    if hidenametwoTOGGLE then
        return "🟢 𝗛𝗜𝗗𝗘𝗡𝗔𝗠𝗘"
    else
        return "🔴 𝗛𝗜𝗗𝗘𝗡𝗔𝗠𝗘"
    end
end

function maxfps()
    if upgradefpsTOGGLE then
        return "🟢 𝗨𝗣𝗚𝗥𝗔𝗗𝗘 𝗙𝗣𝗦"
    else
        return "🔴 𝗨𝗣𝗚𝗥𝗔𝗗𝗘 𝗙𝗣𝗦"
    end
end


function minimize_script()
    gg.toast("Minimizing script")
    while true do
        if gg.isVisible(true) then
            gg.setVisible(false)
            break
        end
        gg.sleep(100)
    end
end

minimize_script()
checkArchitecture()

while true do
    local menu = gg.choice({
        "𝗖𝗟𝗢𝗦𝗘 𝗠𝗘𝗡𝗨",
        "𝗘𝗫𝗜𝗧 𝗟𝗢𝗔𝗗𝗘𝗥",
        "𝗥𝗘𝗦𝗘𝗧 𝗠𝗘𝗡𝗨",
        "⬇️🅸🅽 🅶🅰🅼🅴 🅼🅴🅽🆄 ⬇️\n" ..
        bypasscheat(),
        bypassreport(),
        bypasslobby(),
        "⬇️🅸🅽 🅶🅰🅼🅴 🅼🅴🅽🆄 ⬇️\n" ..
        drone(),
        mhyes(),
        mhnovtwo(),
        fixgrass(),
        hidenametwo(),
        maxfps(),
        "𝗥𝗘𝗦𝗘𝗧 𝗕𝗔𝗡𝗡𝗘𝗗 𝗔𝗖𝗖𝗢𝗨𝗡𝗧"
    }, nil, "𝐈𝐓𝐀𝐂𝐇𝐈 𝐌𝐎𝐃𝐙 𝐌𝐋𝐁𝐁 𝐕𝐈𝐏\n𝐕𝐞𝐫𝐬𝐢𝐨𝐧 = 𝟏.𝟖.𝟗𝟐.𝟗𝟕𝟎𝟐\n𝐏𝐚𝐜𝐤𝐚𝐠𝐞 𝐍𝐚𝐦𝐞 = 𝐌𝐨𝐛𝐢𝐥𝐞 𝐋𝐞𝐠𝐞𝐧𝐝 𝐔𝐧𝐢𝐭𝐲𝐊𝐢𝐥𝐥𝐬𝐌𝐞\n𝐒𝐭𝐚𝐭𝐮𝐬 = 𝐒𝐚𝐟𝐞")
    
    if menu == 1 then
        minimize_script()
    elseif menu == 2 then
        gg.toast("Exiting script")
        bypasscheatOFF()
        bypassreportOFF()
        bypasslobbyOFF()
        droneOFF()
        mhyesOFF()
        mhnovtwoOFF()
        fixgrassOFF()
        hidenametwo()OFF()
        maxfpsOFF()
        os.exit()
    elseif menu == 3 then
        bypasscheatOFF()
        bypassreportOFF()
        bypasslobbyOFF()
        droneOFF()
        mhyesOFF()
        mhnovtwoOFF()
        fixgrassOFF()
        offnameOFF()
        maxfpsOFF()
    elseif menu == 4 then
        if bypasscheatTOGGLE then
            bypasscheatOFF()
        else
            bypasscheatON()
        end
    elseif menu == 5 then
        if bypassreportTOGGLE then
            bypassreportOFF()
        else
            bypassreportON()
        end
    elseif menu == 6 then
        if bypasslobTOGGLE then
            bypasslobbyOFF()
        else
            bypasslobbyON()
        end
    elseif menu == 7 then
        if dropviewTOGGLE then
            droneOFF()
        else
            droneON()
        end
    elseif menu == 8 then
        if mhyesTOGGLE then
            mhyesOFF()
        else
            mhyesON()
        end
    elseif menu == 9 then
        if mhnovtwoTOGGLE then
            mhnovtwoOFF()
        else
            mhnovtwoON()
        end
    elseif menu == 10 then
        if fixgrassTOGGLE then
            fixgrassOFF()
        else
            fixgrassON()
        end
    elseif menu == 11 then
        if hidenametwoTOGGLE then
            hidenametwoOFF()
        else
            hidenametwoON()
        end
    elseif menu == 12 then
        if upgradefpsTOGGLE then
            maxfpsOFF()
        else
            maxfpsON()
        end
    elseif menu == 13 then
        lololo()
        end
        end
   
