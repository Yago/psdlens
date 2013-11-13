#!/usr/bin/env ruby

require 'optparse'
require 'fileutils'
require 'psd'
require 'json'

puts "==================================="


# Create json reports for all PSD files
def globalReport(psdFiles, base)
  psdFiles.each do |psdFile|
    name = File.basename(psdFile, ".psd")
    PSD.open(psdFile) do |psd|
      content = psd.tree.to_hash
      jsonContent = JSON.pretty_generate(content)
      file = File.join(base, "#{name}.json")
      File.open(file, "w") { |f| f.write(jsonContent) }
      puts "[create] #{file}"
    end
  end
end


# Show all used fonts
def fontReport(psdFiles)
  fontData = Array.new

  psdFiles.each do |psdFile|
    name = File.basename(psdFile)
    PSD.open(psdFile) do |psd|
      puts "\n[ #{name} ] Font used =================================== \n\n"
      psd.layers.each do |layer|
        fontRaw = layer.text
        fontString = fontRaw.to_s()
        fontstring = fontString.gsub!(':font=>{:name=>"', "spliter")
        fontstring = fontString.gsub!('", :sizes=>[', "spliter")
        fontArray = fontString.split("spliter")
        fontName = fontArray[1].to_s()
        if fontName == ""
        else
          fontData.push("* " + fontName)
        end
      end
    end
    fontDataClean = fontData.uniq
    fontDataClean.each do |d|
      puts d
    end
  end
end


# Extract text content from PSD and display font-size, font-family and color
def textReport(psdFiles)
  textData = Array.new

  psdFiles.each do |psdFile|
    name = File.basename(psdFile)
    PSD.open(psdFile) do |psd|
      puts "\n[ #{name} ] Text Report =================================== \n\n"
      psd.layers.each do |layer|
        textRaw = layer.adjustments[:type]
        textString = textRaw.to_s()

        fontRaw = layer.text
        fontString = fontRaw.to_s()
        fontstring = fontString.gsub!(':font=>{:name=>"', "spliter")
        fontstring = fontString.gsub!('", :sizes=>[', "spliter")
        fontstring = fontString.gsub!('], :colors=>[', "spliter")
        fontstring = fontString.gsub!(';\ncolor: ', "spliter")
        fontstring = fontString.gsub!(';"}, :left=>', "spliter")
        fontArray = fontString.split("spliter")

        fontName = fontArray[1].to_s()
        fontSize = fontArray[2].to_s()
        fontColor = fontArray[4].to_s()

        if textString == ""
        else
          textData.push("\n#{textString}\n=> #{fontName} | #{fontSize}px | #{fontColor}")
        end
      end
    end
    textDataClean = textData.uniq
    textDataClean.each do |d|
      puts d
    end
  end
end

# Extract text content from PSD and display font-size, font-family and color
def styleReport(psdFiles)
  textData = Array.new

  psdFiles.each do |psdFile|
    name = File.basename(psdFile)
    PSD.open(psdFile) do |psd|
      puts "\n[ #{name} ] Text Report =================================== \n\n"
      psd.layers.each do |layer|
        textRaw = layer.adjustments[:type]
        textString = textRaw.to_s()

        fontRaw = layer.text
        fontString = fontRaw.to_s()
        fontstring = fontString.gsub!(':font=>{:name=>"', "spliter")
        fontstring = fontString.gsub!('", :sizes=>[', "spliter")
        fontstring = fontString.gsub!('], :colors=>[', "spliter")
        fontstring = fontString.gsub!(';\ncolor: ', "spliter")
        fontstring = fontString.gsub!(';"}, :left=>', "spliter")
        fontArray = fontString.split("spliter")

        fontName = fontArray[1].to_s()
        fontSize = fontArray[2].to_s()
        fontColor = fontArray[4].to_s()

        if textString == ""
        else
          textData.push("#{fontName} | #{fontSize}px | #{fontColor}")
        end
      end
    end
    textDataClean = textData.uniq
    textDataClean.each do |d|
      puts d
    end
  end
end


# Extract text content from PSD
def contentReport(psdFiles)
  textData = Array.new

  psdFiles.each do |psdFile|
    name = File.basename(psdFile)
    PSD.open(psdFile) do |psd|
      puts "\n[ #{name} ] Text Content =================================== \n\n"
      psd.layers.each do |layer|
        textRaw = layer.adjustments[:type]
        textString = textRaw.to_s()
        if textString == ""
        else
          textData.push("\n#{textString}\n")
        end
      end
    end
    textDataClean = textData.uniq
    textDataClean.each do |d|
      puts d
    end
  end
end


def metaReport(psdFiles)
  fontData = Array.new

  psdFiles.each do |psdFile|
    name = File.basename(psdFile)
    PSD.open(psdFile) do |psd|
      puts "\n[ #{name} ] Meta report =================================== \n\n"
      psdWidth = psd.width
      psdHeight = psd.height
      psdLayers = psd.layers.size
      psdGroup = psd.folders.size
      psdMode = psd.header.mode_name
      psd.layers.each do |layer|
        fontRaw = layer.text
        fontString = fontRaw.to_s()
        fontArray = fontString.split(':font=>{:name=>"')
        fontFirstPart = fontArray[1].to_s()
        fontNameArray = fontFirstPart.split('", :sizes=>[')
        fontName = fontNameArray[0].to_s()
        if fontName == ""
        else
          fontData.push(fontName)
        end
      end
      fontDataClean = fontData.uniq
      psdFonts = fontDataClean.to_s().tr('[]""', '')
      puts "Document size :   #{psdWidth}px x #{psdHeight}px"
      puts "Color mode :      #{psdMode}"
      puts "Content :         #{psdLayers} layers in #{psdGroup} groups"
      puts "Fonts :           #{psdFonts}"
    end
  end
end


# Parse options and creat doc
OptionParser.new do |opts|
  opts.banner = "
Usage:            #{File.basename($0)} [path] [method]

[path] :          Absolute path or current location using .

[method] :

  * meta          Display all informations like size, color mode or used fontString
  * text          Display text content from all PSD files and return font-family, font-size and color
  * font          Display only used font
  * style         Display font typologies
  * content       Display only text content
  * report        Create a complete json report for all PSD files 

Example :         #{File.basename($0)} . meta

"
  begin
    opts.parse!(ARGV)
  rescue OptionParser::ParseError => e
    warn e.message
    puts opts
    exit 1
  end
end

if ARGV.empty?
  abort "[error] Please specify the PSD directory to parse, e.g. `#{File.basename($0)} .'"
elsif !File.exists?(ARGV[0])
  abort "[error] '#{ARGV[0]}' does not exist.\n\n"
elsif !File.directory?(ARGV[0])
  abort "[error]  '#{ARGV[0]}' is not a directory."
end

path = ARGV[0]
methodArg = ARGV[1]

currentPath = Dir.pwd + "/"
base = path == "." ? currentPath : path + "/"
psdFiles = Dir[base+"*.psd"]

beginMessage = "[start] Begin PSD analysis ...\n\n"


case methodArg
when "meta"
  puts beginMessage
  metaReport(psdFiles)
when "text"
  puts beginMessage
  textReport(psdFiles)
when "style"
  puts beginMessage
  styleReport(psdFiles)
when "content"
  puts beginMessage
  contentReport(psdFiles)
when "font"
  puts beginMessage
  fontReport(psdFiles)
when "report"
  puts beginMessage
  globalReport(psdFiles, base)
else
  abort "\n[error] no correct method select\n\n"
end


puts "\n\n[done] PSD well analyzed !\n\n"











