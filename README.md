local mhyesTOGGLE = false
local mhnoTOGGLE = false
local fixgrassTOGGLE = false
local unlockallskinTOGGLE = false
local unlockspellTOGGLE = false
local showjungleTOGGLE = false
local nocdTOGGLE = false
local mhnovtwoTOGGLE = false

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
    gg.alert("Vip Script By Mgubs")
    gg.alert("Script Expired On September 1,2024")
    if arch then
        gg.alert("You Are Running On 64Bit")
        onA = "-2,999,674,700,646,286,368"
    else
        gg.alert("You Are Running On 32Bit")
        onA = "-2,220,275,583,130,009,600"
    end
end

function setValues(address, flags, value) gg.setValues({[1] = {address = address, flags = flags, value = value}}) end
local running = true
function setvalue(address,flags,value)
local tt={} tt[1]={} tt[1].address=address tt[1].flags=flags tt[1].value=value gg.setValues(tt) end
function split(szFullString, szSeparator)
    local nSplitArray = {}
    local nFindStartIndex = 1
    local nSplitIndex = 1
    
    while true do
        local nFindLastIndex = string.find(szFullString, szSeparator, nFindStartIndex)
        
        if not nFindLastIndex then
            nSplitArray[nSplitIndex] = string.sub(szFullString, nFindStartIndex, string.len(szFullString))
            break
        end
        
        nSplitArray[nSplitIndex] = string.sub(szFullString, nFindStartIndex, nFindLastIndex - 1)
        nFindStartIndex = nFindLastIndex + string.len(szSeparator)
        nSplitIndex = nSplitIndex + 1
    end
    
    return nSplitArray
end

function setValue(libX , regionX , addressX , flagsX , valueX)
getLib = gg.getRangesList(libX)
for name , lib in ipairs(getLib) do
if lib.state == regionX then Region = name break end
end
if Region == nil then gg.toast("Not found") return nil end
val = {{address = getLib[Region].start + addressX , flags = flagsX , value = valueX, freeze = true}}
gg.addListItems(val)
gg.clearList()
return true
end

hexTrue = "D6 5F 03 C0 D2 80 00 20h"
hexFalse = "D6 5F 03 C0 D2 80 00 00h"

function xgxc(szpy, qmxg)
    for x = 1, #qmxg do
        local xgpy = szpy + qmxg[x]["offset"]
        local xglx = qmxg[x]["type"]
        local xgsz = qmxg[x]["value"]
        local xgdj = qmxg[x]["freeze"]
        
        if xgdj == nil or xgdj == "" then
            gg.setValues({[1] = {address = xgpy, flags = xglx, value = xgsz}})
        else
            gg.addListItems({[1] = {address = xgpy, flags = xglx, freeze = xgdj, value = xgsz}})
        end
    end
end
function xqmnb(qmnb)
gg.clearResults()
    gg.clearResults()
    gg.setRanges(qmnb[1]["memory"])
    gg.searchNumber(qmnb[3]["value"], qmnb[3]["type"])
    
    if gg.getResultCount() > 0 then
        gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"])
        gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"])
        gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"])
        
        local sl = gg.getResults(999999)
        local sz = gg.getResultCount()
        
        if sz > 999999 then
            sz = 999999
        end
        
        for i = 1, sz do
            local pdjg = true
            for v = 4, #qmnb do
                local pysz = {}
                pysz[1] = {}
                pysz[1].address = sl[i].address + qmnb[v]["offset"]
                pysz[1].flags = qmnb[v]["type"]
                local szpy = gg.getValues(pysz)
                local pdpd = qmnb[v]["lv"] .. ";" .. szpy[1].value
                local szpd = split(pdpd, ";")
                local tzszpd = szpd[1]
                local pyszpd = szpd[2]
                
                if tzszpd ~= pyszpd then
                    pdjg = false
                    break
                end
            end
            
            if pdjg then
                xgxc(sl[i].address, qmxg)
            end
        end
    end
end
function SearchWrite(Search, Write, Type)
    gg.clearResults()
    gg.setVisible(false)
    gg.searchNumber(Search[1][1], Type)
    
    local count = gg.getResultCount()
    local result = gg.getResults(count)
    gg.clearResults()
    
    local data = {}
    local base = Search[1][2]
    
    if count > 0 then
        for k = 2, #Search do
            local tmp = {}
            local offset = Search[k][2] - base
            local num = Search[k][1]
            
            for i, v in ipairs(result) do
                tmp[#tmp+1] = {}
                tmp[#tmp].address = v.address + offset
                tmp[#tmp].flags = v.flags
            end
            
            tmp = gg.getValues(tmp)
            
            for i, v in ipairs(tmp) do
                if tostring(v.value) ~= tostring(num) then
                    result[i].isUseful = false
                end
            end
        end
        
        for i, v in ipairs(result) do
            if v.isUseful then
                data[#data+1] = v.address
            end
        end
        
        if #data > 0 then
            local t = {}
            local base = Search[1][2]
            
            for i = 1, #data do
                for k, w in ipairs(Write) do
                    local offset = w[2] - base
                    t[#t+1] = {}
                    t[#t].address = data[i] + offset
                    t[#t].flags = Type
                    t[#t].value = w[1]
                    
                    if w[3] == true then
                        local item = {}
                        item[#item+1] = t[#t]
                        item[#item].freeze = true
                        gg.addListItems(item)
                    end
                end
            end
            
            gg.setValues(t)
            gg.addListItems(t)
            return true
        else
            return false
        end
    else
        return false
    end
end
 
           function split(A0_2269, A1_2270)
    for _FORV_6_ in (A0_2269 .. A1_2270):gmatch(_UPVALUE0_("282e2d29") .. A1_2270) do
      table.insert({}, _FORV_6_)
    end 
 
    return {}
  end 
 
  
  function setValuesOrFreeze(A0_2271, A1_2272)
    local L2_2273
    L2_2273 = {}
    for _FORV_6_ = 1, #A1_2272 do
      if A1_2272[_FORV_6_].freeze then
        table.insert(L2_2273, {address = A0_2271 + A1_2272[_FORV_6_].offset, flags = A1_2272[_FORV_6_].type, value = A1_2272[_FORV_6_].value, ["freeze"] = A1_2272[_FORV_6_].freeze})
      else
gg.setValues({{address = A0_2271 + A1_2272[_FORV_6_].offset, flags = A1_2272[_FORV_6_].type, value = A1_2272[_FORV_6_].value, ["freeze"] = A1_2272[_FORV_6_].freeze}})
      end 
 
    end 
 
    if #L2_2273 > 0 then
      gg.addListItems(L2_2273)
    end 
 
  end 
 
  
  function xqmnb(A0_2274)
    gg["clearResults"]()
    gg["setRanges"](A0_2274[1].memory)
    gg["searchNumber"](A0_2274[3].value, A0_2274[3].type)
    if gg.getResultCount() == 0 then
      return
    end 
 
    gg["refineNumber"](A0_2274[3].value, A0_2274[3].type)
    if gg.getResultCount() == 0 then
      return
    end 
 
    for _FORV_6_ = 1, #gg["getResults"](math.min(gg.getResultCount(), 10000)) do
      for _FORV_11_ = 4, #A0_2274 do
        if true and split(A0_2274[_FORV_11_].lv .. ";" .. gg.getValues({{address = gg["getResults"](math.min(gg.getResultCount(), 10000))[_FORV_6_].address + A0_2274[_FORV_11_].offset, flags = A0_2274[_FORV_11_].type}})[1].value, ";")[1] ~= split(A0_2274[_FORV_11_].lv .. ";" .. gg.getValues({{address = gg["getResults"](math.min(gg.getResultCount(), 10000))[_FORV_6_].address + A0_2274[_FORV_11_].offset, flags = A0_2274[_FORV_11_].type}})[1].value, ";")[2] then
        end 
 
      end 
 
      if false then
        setValuesOrFreeze(gg["getResults"](math.min(gg.getResultCount(), 10000))[_FORV_6_].address, qmxg)
      end 
 
    end 
 
  end 
 
  
  function searchWrite(A0_2275, A1_2276, A2_2277)
    gg["clearResults"]()
    gg["searchNumber"](A0_2275[1][1], A2_2277)
    if gg.getResultCount() == 0 then
      return false
    end 
 
    for _FORV_9_, _FORV_10_ in ipairs((gg["getResults"](math.min(gg.getResultCount(), 10000)))) do
      _FORV_10_.isUseful = true
    end 
 
    for _FORV_9_ = 2, #A0_2275 do
      for _FORV_14_, _FORV_15_ in ipairs((gg["getResults"](math.min(gg.getResultCount(), 10000)))) do
        if _FORV_15_.isUseful and tostring(gg.getValues({{address = _FORV_15_.address + (A0_2275[_FORV_9_][2] - A0_2275[1][2]), flags = _FORV_15_.flags}})[1].value) ~= tostring(A0_2275[_FORV_9_][1]) then
          _FORV_15_.isUseful = false
        end 
 
      end 
 
    end 
 
    for _FORV_9_, _FORV_10_ in ipairs((gg["getResults"](math.min(gg.getResultCount(), 10000)))) do
      if _FORV_10_.isUseful then
        table.insert({}, _FORV_10_.address)
      end 
 
    end 
 
    if #{} > 0 then
      for _FORV_10_, _FORV_11_ in ipairs({}) do
        for _FORV_15_, _FORV_16_ in ipairs(A1_2276) do
          if _FORV_16_[3] then
    gg.addListItems({{address = _FORV_11_ + (_FORV_16_[2] - A0_2275[1][2]), flags = A2_2277, value = _FORV_16_[1], ["freeze"] = true}})
          end 
 
          table.insert({}, {address = _FORV_11_ + (_FORV_16_[2] - A0_2275[1][2]), flags = A2_2277, value = _FORV_16_[1], ["freeze"] = true})
        end 
 
      end 
 
      gg.setValues({})
      gg.addListItems({})
    else
      return false
    end 
 
  end 
 
  
  function setvalue(A0_2278, A1_2279, A2_2280)
    local L3_2281
    L3_2281 = {}
    L3_2281[1] = {}
    L3_2281[1].address = A0_2278
    L3_2281[1].flags = A1_2279
    L3_2281[1].value = A2_2280
    gg.setValues(L3_2281)
  end 


    
function xgxc(A0_27, A1_28)

local L2_29, L3_30, L4_31, L5_32
L2_29 = 1
L3_30 = #A1_28
for _FORV_5_ = 1, #A1_28 do 
xgpy = A0_27 + A1_28[_FORV_5_].offset
xglx = A1_28[_FORV_5_].type
xgsz = A1_28[_FORV_5_].value
gg.setValues({
[1] = {
address = xgpy,
flags = xglx,
value = xgsz
}
})
xgsl = xgsl + 1
end 

end 


function xqmnb(A0_33)
gg.clearResults()
gg.setRanges(A0_33[1].memory)
gg.searchNumber(A0_33[3].value, A0_33[3].type)
if gg.getResultCount() == 0 then
gg.toast(A0_33[2].name .. " Active")
else
gg.refineNumber(A0_33[3].value, A0_33[3].type)
gg.refineNumber(A0_33[3].value, A0_33[3].type)
gg.refineNumber(A0_33[3].value, A0_33[3].type)
if gg.getResultCount() == 0 then
gg.toast(A0_33[2].name .. " Active")
else
sl = gg.getResults(999999)
sz = gg.getResultCount()
xgsl = 0
if 999999 < sz then
sz = 999999
end 

for _FORV_4_ = 1, sz do 
pdsz = true
for _FORV_8_ = 4, #A0_33 do 
if pdsz == true then
pysz = {}
pysz[1] = {}
pysz[1].address = sl[_FORV_4_].address + A0_33[_FORV_8_].offset
pysz[1].flags = A0_33[_FORV_8_].type
szpy = gg.getValues(pysz)
pdpd = A0_33[_FORV_8_].lv .. ";" .. szpy[1].value
szpd = split(pdpd, ";")
tzszpd = szpd[1]
pyszpd = szpd[2]
if tzszpd == pyszpd then
pdjg = true
pdsz = true
else
pdjg = false
pdsz = false
end 

end 

end 

if pdjg == true then
szpy = sl[_FORV_4_].address
xgxc(szpy, qmxg)
xgjg = true
end 

end 

if xgjg == true then
gg.toast(A0_33[2].name .. " " .. xgsl)
else
gg.toast(A0_33[2].name .. " Xong")
end 

end 

end 

end 



gg.sleep("100")
LuaLibraryTool = -1
function HOME()
MENU = gg.choice({
        "🔸𝗕𝗬𝗣𝗔𝗦𝗦 𝗔𝗡𝗧𝗜 𝗖𝗛𝗘𝗔𝗧🔸", --1
        "🔸𝗕𝗬𝗣𝗔𝗦𝗦 𝗔𝗡𝗧𝗜 𝗥𝗘𝗣𝗢𝗥𝗧🔸", --2
        "🔸𝗕𝗬𝗣𝗔𝗦𝗦 𝗟𝗢𝗕𝗕𝗬🔸", --3
        "🔸𝗕𝗬𝗣𝗔𝗦𝗦 𝗟𝗢𝗚𝗢🔸", --4
        "⬇️🅸🅽 🅶🅰🅼🅴 🅼🅴🅽🆄 ⬇️", --5
        "🔸𝗠𝗔𝗣𝗛𝗔𝗖𝗞🔸", --5
        "🔸𝗠𝗔𝗣𝗛𝗔𝗖𝗞 𝗡𝗢 𝗜𝗖𝗢𝗡🔸", --6
        "🔸𝗙𝗜𝗫 𝗚𝗥𝗔𝗦𝗦🔸", --7
        "🔸𝗗𝗥𝗢𝗡𝗘 𝗩𝗜𝗘𝗪🔸", --8
        "🔸LEAVE SCRIPT 🔸" --EndScript
}, nil, "𝐈𝐓𝐀𝐂𝐇𝐈 𝐌𝐎𝐃𝐙 𝐌𝐋𝐁𝐁 𝐕𝐈𝐏\n𝐕𝐞𝐫𝐬𝐢𝐨𝐧 = 𝟏.𝟖.𝟗𝟐.𝟗𝟕𝟎𝟐\n𝐏𝐚𝐜𝐤𝐚𝐠𝐞 𝐍𝐚𝐦𝐞 = 𝐌𝐨𝐛𝐢𝐥𝐞 𝐋𝐞𝐠𝐞𝐧𝐝 𝐔𝐧𝐢𝐭𝐲𝐊𝐢𝐥𝐥𝐬𝐌𝐞\n𝐒𝐭𝐚𝐭𝐮𝐬 = 𝐒𝐚𝐟𝐞")
if MENU == nil then
  else
   if MENU == 1 then --choice
      MENU1()
     end
   if MENU == 2 then --choice
      MENU2()
     end
   if MENU == 3 then --choice
      MENU3()
     end
   if MENU == 4 then --choice
      MENU4()
     end
     if MENU == 5 then --choice
      MENU5()
     end
     if MENU == 6 then --choice
      MENU6()
     end
     if MENU == 7 then --choice
      MENU7()
     end
     if MENU == 8 then --choice
      MENU8()
     end
     if MENU == 9 then --choice
      MENU9()
     end
    if MENU == 10 then --EndScript
      LOBBY()
     end
   end
  LuaLibraryTool = -1
end

function MENU1() --choice
gg.toast("𝐀𝐍𝐓𝐈 𝐂𝐇𝐄𝐀𝐓 𝐀𝐂𝐓𝐈𝐕𝐀𝐓𝐄")
end

function MENU2() --choice
local LibStart = gg.getRangesList('liblogic.so')[1].start
gg.setRanges(gg.REGION_CODE_APP)
gg.searchNumber(":cheat", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ -- table(d920f3c)
	[1] = { -- table(c3cf4c5)
		['address'] = 3171953268.0,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 10,
	},
})
gg.searchNumber(":SDKReport", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":strTraceLog", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":GetTraceLog", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":mainTracePath", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":md5", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":MD5", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":kill", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":Cache", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":anti", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":login", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.clearResults()
gg.searchNumber(":terminate", gg.TYPE_WORD, false, gg.SIGN_EQUAL, 0, -1)
gg.getResults(9999)
gg.setValues({ 
	[1] = { 
		['address'] = 3171953268.0,
		['flags'] = 4, 
		['value'] = 10,
	},
})
gg.setValues({ -- table(b1c1716)
	[1] = { -- table(b02b897)
		['address'] = 0x7fff3718dc1c,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(6196323)
	[1] = { -- table(5161020)
		['address'] = 0x7fff3718deac,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(5f6139b)
	[1] = { -- table(8a1c338)
		['address'] = 0x7fff3718e438,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(5bf198b)
	[1] = { -- table(8a86a68)
		['address'] = 0x7fff3718ef78,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(cff3d98)
	[1] = { -- table(901b9f1)
		['address'] = 0x7fff3718f560,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(93cc8)
	[1] = { -- table(f240161)
		['address'] = 0x7fff3718fa18,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(49d2d3)
	[1] = { -- table(fdd3e10)
		['address'] = 0x7fff371904a8,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(582bf28)
	[1] = { -- table(fd9c441)
		['address'] = 0x7fff37190b44,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(6784258)
	[1] = { -- table(d7b3fb1)
		['address'] = 0x7fff37191030,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(a0532a3)
	[1] = { -- table(6b669a0)
		['address'] = 0x7fff371916ac,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(fc3ccb8)
	[1] = { -- table(d0e6a91)
		['address'] = 0x7fff37191dc8,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(63390a6)
	[1] = { -- table(6b5ae7)
		['address'] = 0x7fff37191eac,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 

gg.setValues({ -- table(9d08571)
	[1] = { -- table(cd76f56)
		['address'] = 0x7fff37192140,
		['flags'] = 4, -- gg.TYPE_DWORD
		['value'] = 'h C0 03 5F D6',
	},
}) 
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
gg.alert("𝐁𝐘𝐏𝐀𝐒𝐒 𝐋𝐎𝐁𝐁𝐘 𝐀𝐂𝐓𝐈𝐕𝐀𝐓𝐄")
end

function MENU3() --choice
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

function MENU4()
HexPatches.ITACHI("libnativelogin.so",0xb9e3c,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0xba1e4,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0xba2ac,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0x2b66c,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0x2b6ec,"h00 00 80 D2 C0 03 5F D6",32);
HexPatches.ITACHI("libnativelogin.so",0x2b7c4,"h00 00 80 D2 C0 03 5F D6",32);
gg.alert("𝐀𝐍𝐓𝐈 𝐂𝐇𝐄𝐀𝐓 𝐀𝐂𝐓𝐈𝐕𝐀𝐓𝐄")
end

function MENU5()
end

function MENU6()
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
    gg.toast("𝐌𝐀𝐏𝐇𝐀𝐂𝐊 𝐀𝐂𝐓𝐈𝐕𝐀𝐓𝐄")
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

function MENU7()
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

function MENU8()
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

function MENU9()

end

function LOBBY() --End Script
gg.alert("ITACHI MLBB SCRIPT")
print("Created By Lua Library Tool")
gg.skipRestoreState()
  gg.setVisible(true)
  os.exit()
end
while true do
  if gg.isVisible(true) then
    LuaLibraryTool = 1
    gg.setVisible(false)
  end
  if LuaLibraryTool == 1 then
    HOME()
  end
end

--Created By Lua Library Tool
--Lua Library Tool By CyraX Gaming
